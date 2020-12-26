---
layout: post
title: "[CodeStudy] HPWE and Its Interfaces between Hardware and Software"
description: "This article introduces the MMIO register files of HWPE in Pulp SoC with its related c codes for simulation. It also gives hints of custom modifying the codes to use more registers or more events. I think it can also help you to understand the interaction between hardware and software."
categories: [CodeStudy]
tags: [HWPE, Pulp, interface]
last_updated: 2020-12-26 18:44:00 GMT+8
excerpt: "This article introduces the MMIO register files of HWPE in Pulp SoC with its related c codes for simulation. It also gives hints of custom modifying the codes to use more registers or more events. I think it can also help you to understand the interaction between hardware and software."
redirect_from:
  - /2020/12/26/
---

* Kramdown table of contents
{:toc .toc}
# HPWE and Its Interfaces between Hardware and Software

> *Hardware Processing Engines* (HWPEs) are special-purpose, memory-coupled accelerators that can be inserted in the SoC or cluster of a PULP system to amplify its performance and energy efficiency in particular tasks.[^1]

We can use HPWE to accelerate the convolutional computation. However, the official document is not that good, especially when you want to modify some details.

Here, I read the original software c codes and hardware SystemVerilog codes and will introduce the interface between software codes and hardware architecture.

## Register Files in the HWPE

We all know that the ISA is the interface between the software and hardware. In the HWPE, there are several registers stores the data acted as the instructions by MMIO, i.e., the software is able to read and write these register files as they are part of the memory.

![PULPissimo Memory-map](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20201226164152.png)

There are 25 registers in the HWPE from the codes, in which $6^{th}$, $7^{th}$ and $15^{th}$ registers are reserved and can be custom utilized.

| reg   | offset     | bits      | bitmask        | content                    |
| ----- | ---------- | --------- | -------------- | -------------------------- |
| 0     | 0x0000     | 31: 0     | 0xffffffff     | TRIGGER                    |
| 1     | 0x0004     | 31: 0     | 0xffffffff     | ACQUIRE                    |
| 2     | 0x0008     | 31: 0     | 0xffffffff     | EVT_ENABLE                 |
| 3     | 0x000c     | 31: 0     | 0xffffffff     | STATUS                     |
| 4     | 0x0010     | 31: 0     | 0xffffffff     | RUNNING_JOB                |
| 5     | 0x0014     | 31: 0     | 0xffffffff     | SOFT_CLEAR                 |
| **6-7** |            | **31: 0** | **0xffffffff** | **Reserved**               |
| 8     | 0x0020     | 31: 0     | 0xffffffff     | BYTECODE0 [31:0]           |
| 9     | 0x0024     | 31: 0     | 0xffffffff     | BYTECODE1 [63:32]          |
| 10    | 0x0028     | 31: 0     | 0xffffffff     | BYTECODE2 [95:64]          |
| 11    | 0x002c     | 31: 0     | 0xffffffff     | BYTECODE3 [127:96]         |
| 12    | 0x0030     | 31: 0     | 0xffffffff     | BYTECODE4 [159:128]        |
| 13    | 0x0034     | 31:16     | 0xffff0000     | LOOPS0    [15:0]           |
|       |            | 15: 0     | 0x0000ffff     | BYTECODE5 [175:160]        |
| 14    | 0x0038     | 31: 0     | 0xffffffff     | LOOPS1    [47:16]          |
| **15** |            | **31: 0** | **0xffffffff** | **Reserved**               |
| 0     | 0x0040     | 31: 0     | 0xffffffff     | A_ADDR                     |
| 1     | 0x0044     | 31: 0     | 0xffffffff     | B_ADDR                     |
| 2     | 0x0048     | 31: 0     | 0xffffffff     | C_ADDR                     |
| 3     | 0x004c     | 31: 0     | 0xffffffff     | D_ADDR                     |
| 4     | 0x0050     | 31: 0     | 0xffffffff     | NB_ITER                    |
| 5     | 0x0054     | 31: 0     | 0xffffffff     | LEN_ITER                   |
| 6     | 0x0058     | 31:16     | 0xffff0000     | SHIFT                      |
|       |            | 0: 0      | 0x00000001     | SIMPLEMUL                  |
| 7     | 0x005c     | 31: 0     | 0xffffffff     | VECTSTRIDE                 |
| 8     | 0x0060     | 31: 0     | 0xffffffff     | VECTSTRIDE2                |

## Parameters for the HWPE: stored in hwpe_ctrl_regfile

### Configurations and Defines

The parameters are defined in the file `hwpe_ctrl_package.sv`.

```verilog
// file: hwpe_ctrl_package.sv

package hwpe_ctrl_package;

  parameter int unsigned REGFILE_N_CORES            = 8;
  parameter int unsigned REGFILE_N_CONTEXT          = 2;
  parameter int unsigned REGFILE_N_EVT              = 2;
  parameter int unsigned REGFILE_N_REGISTERS        = 64;
  parameter int unsigned REGFILE_N_MANDATORY_REGS   = 7;
  parameter int unsigned REGFILE_N_MAX_IO_REGS      = 48;
  parameter int unsigned REGFILE_N_MAX_GENERIC_REGS = 8;
  parameter int unsigned REGFILE_N_RESERVED_REGS    = REGFILE_N_REGISTERS-REGFILE_N_MANDATORY_REGS-REGFILE_N_MAX_GENERIC_REGS-REGFILE_N_MAX_IO_REGS;

```

And the parameters related to the jobs are defined in the file `mac_package.sv`.

```verilog
// file: mac_package.sv

package mac_package;

  // registers in register file
  parameter int unsigned MAC_REG_A_ADDR           = 0;
  parameter int unsigned MAC_REG_B_ADDR           = 1;
  parameter int unsigned MAC_REG_C_ADDR           = 2;
  parameter int unsigned MAC_REG_D_ADDR           = 3;
  parameter int unsigned MAC_REG_NB_ITER          = 4;
  parameter int unsigned MAC_REG_LEN_ITER         = 5;
  parameter int unsigned MAC_REG_SHIFT_SIMPLEMUL  = 6;
  parameter int unsigned MAC_REG_SHIFT_VECTSTRIDE = 7;
```

These parameters is shown in the table above.

Related parameters in the software codes are defined in the file `archi/hwme_v1.h`:

```c
// file: archi/hwme_v1.h

#define HWME_TRIGGER          0x00
#define HWME_ACQUIRE          0x04
#define HWME_FINISHED         0x08
#define HWME_STATUS           0x0c
#define HWME_RUNNING_JOB      0x10
#define HWME_SOFT_CLEAR       0x14

#define HWME_BYTECODE         0x20
#define HWME_BYTECODE0_OFFS        0x00
#define HWME_BYTECODE1_OFFS        0x04
#define HWME_BYTECODE2_OFFS        0x08
#define HWME_BYTECODE3_OFFS        0x0c
#define HWME_BYTECODE4_OFFS        0x10
#define HWME_BYTECODE5_LOOPS0_OFFS 0x14
#define HWME_LOOPS1_OFFS           0x18

#define HWME_A_ADDR          0x40
#define HWME_B_ADDR          0x44
#define HWME_C_ADDR          0x48
#define HWME_D_ADDR          0x4c
#define HWME_NB_ITER         0x50
#define HWME_LEN_ITER        0x54
#define HWME_SHIFT_SIMPLEMUL 0x58
#define HWME_VECTSTRIDE      0x5c
#define HWME_VECTSTRIDE2     0x60
```

And the data are stored in the module `hwpe_ctrl_regfile`:

```verilog
// file: hwpe_ctrl_regfile.sv

logic [N_CONTEXT-1:0] [N_REGISTERS-1:N_MANDATORY_REGS+N_RESERVED_REGS+N_MAX_GENERIC_REGS] [31:0] regfile_mem;
logic [N_MANDATORY_REGS-1:2] [31:0] regfile_mem_mandatory;
logic [N_MANDATORY_REGS+N_RESERVED_REGS+N_GENERIC_REGS-1:N_MANDATORY_REGS+N_RESERVED_REGS] [31:0] regfile_mem_generic;
```

### Write via MMIOs

The address of the writing registers is `regfile_in.addr`. The port `cfg.add` comes from the top module.

```verilog
// file: hwpe_ctrl_slave.sv

generate
    logic [LOG_REGS_MC-LOG_REGS-1:0] context_addr;
    assign context_addr = cfg.add[LOG_REGS_MC+2:LOG_REGS+2] - 1;

    if(N_CONTEXT>1) begin : multi_context_gen
        always_comb
            begin
                if(~cfg.wen) begin
                    regfile_in.addr = {pointer_context, cfg.add[LOG_REGS+2-1:2]};
                end
                else begin
                    if(cfg.add[LOG_REGS_MC+2:LOG_REGS+2] == '0)
                        regfile_in.addr = {pointer_context, cfg.add[LOG_REGS+2-1:2]};
                    else
                        regfile_in.addr = {context_addr,    cfg.add[LOG_REGS+2-1:2]};
                end
            end
    end
    else begin : single_context_gen
        assign regfile_in.addr = cfg.add[LOG_REGS+2-1:2];
    end

endgenerate
```

Here you can find that only `[LOG_REGS+2-1:2]` bits are used here, just like the address is shifted right 2 bits, i.e, divided by 4.

## Read the data related to the parameters of HWPE

Parameters related to the HWPE are read out via the ports `reg_file.hwpe_params`:

```verilog
// file: hwpe_ctrl_regfile.sv

assign reg_file.hwpe_params = regfile_mem[flags_i.running_context][N_MANDATORY_REGS+N_RESERVED_REGS+N_MAX_GENERIC_REGS+N_IO_REGS-1:N_MANDATORY_REGS+N_RESERVED_REGS+N_MAX_GENERIC_REGS];
```

Later, these params are reorganized in the `mac_ctrl.sv`:

```verilog
// file: mac_ctrl.sv

  /* Direct register file mappings */
  assign static_reg_nb_iter    = reg_file.hwpe_params[MAC_REG_NB_ITER]  + 1;
  assign static_reg_len_iter   = reg_file.hwpe_params[MAC_REG_LEN_ITER] + 1;
  assign static_reg_shift      = reg_file.hwpe_params[MAC_REG_SHIFT_SIMPLEMUL][31:16];
  assign static_reg_simplemul  = reg_file.hwpe_params[MAC_REG_SHIFT_SIMPLEMUL][0];
  assign static_reg_vectstride = reg_file.hwpe_params[MAC_REG_SHIFT_VECTSTRIDE];
  assign static_reg_onestride  = 4;
```

And then are sent to the module `hwpe_ctrl_ucode`:

```verilog
// file: mac_ctrl.sv

  assign ucode_registers_read[MAC_UCODE_MNEM_NBITER]     = static_reg_nb_iter;
  assign ucode_registers_read[MAC_UCODE_MNEM_ITERSTRIDE] = static_reg_vectstride;
  assign ucode_registers_read[MAC_UCODE_MNEM_ONESTRIDE]  = static_reg_onestride;
  assign ucode_registers_read[11:3] = '0;
hwpe_ctrl_ucode #(...) i_ucode (
    ...
    .registers_read_i ( ucode_registers_read )
  );
```

Later, module `hwpe_ctrl_ucode` sends the offset of a, b, and c into the module `mac_fsm` together with the `reg_file_i.hwpe_params`:

```verilog
// file: mac_fsm.sv

ctrl_streamer_o.a_source_ctrl.addressgen_ctrl.base_addr   = reg_file_i.hwpe_params[MAC_REG_A_ADDR] + (flags_ucode_i.offs[MAC_UCODE_A_OFFS]);
...
ctrl_streamer_o.b_source_ctrl.addressgen_ctrl.base_addr   = reg_file_i.hwpe_params[MAC_REG_B_ADDR] + (flags_ucode_i.offs[MAC_UCODE_B_OFFS]);
...
ctrl_streamer_o.c_source_ctrl.addressgen_ctrl.base_addr   = reg_file_i.hwpe_params[MAC_REG_C_ADDR] + (flags_ucode_i.offs[MAC_UCODE_C_OFFS]);
...
ctrl_streamer_o.d_sink_ctrl.addressgen_ctrl.base_addr   = reg_file_i.hwpe_params[MAC_REG_D_ADDR] + (flags_ucode_i.offs[MAC_UCODE_D_OFFS]);
```

## Enable and Disable HWPE by the Software

Two functions are defined at `hal/hwme_v1.h`:

```c
// file: hal/hwme_v1.h

static inline void plp_hwme_enable() {
  *(volatile int*) (ARCHI_SOC_EU_ADDR + (3 << 3)) |=  0xc00;
}

static inline void plp_hwme_disable() {
  *(volatile int*) (ARCHI_SOC_EU_ADDR + (3 << 3)) &= ~0xc00;
}
```

Here `ARCHI_SOC_EU_ADDR + (3 << 3)` actually equals to the address of HWPE.

The parameter `ARCHI_SOC_EU_ADDR1` is defined at `memory_map.h`:

```c
// file: memory_map.h

#define ARCHI_SOC_PERIPHERALS_ADDR    0x1A100000
...
#define ARCHI_SOC_EU_OFFSET           0x00006000
...
#define ARCHI_FC_HWPE_OFFSET          0x0000C000
...
#define ARCHI_SOC_EU_ADDR            ( ARCHI_SOC_PERIPHERALS_ADDR + ARCHI_SOC_EU_OFFSET )
...
#define ARCHI_FC_HWPE_ADDR           ( ARCHI_SOC_PERIPHERALS_ADDR + ARCHI_FC_HWPE_OFFSET )
...
```

## Start to compute in Software and Hardware

The HWPE will start the computation if it receives the trigger signal. In the software codes, this is a function defined in the file `hal/hwme_v1.h`.

```c
// file: hal/hwme_v1.h

#define HWME_ADDR_BASE ARCHI_FC_HWPE_ADDR

#if defined(__riscv__) && !defined(RV_ISA_RV32)
#define HWME_WRITE(value, offset) __builtin_pulp_OffsetedWrite(value, (int *)HWME_ADDR_BASE, offset)
...
#else
#define HWME_WRITE(value, offset) pulp_write32(HWME_ADDR_BASE + (offset), value)
...
#endif

static inline void hwme_trigger_job() {
  HWME_WRITE(0, HWME_TRIGGER);
}
```

And actually, it's writing the $1^{st}$ register file in the HWPE.

From the hardware aspect, two FSMs controls this process.

```verilog
// file: hwpe_ctrl_slave.sv

always_ff @(posedge clk_i or negedge rst_ni)
    begin : pointer_context_proc
        if (rst_ni == 0)
            pointer_context <= 0;
        else if(clear_o == 1'b1)
            pointer_context <= 0;
        else begin
            if (regfile_flags.is_trigger == 1)
                pointer_context <= pointer_context + 1;
        end
    end
...
regfile_flags.is_trigger    = (cfg.req == 1'b1 && cfg.wen == 1'b0 && cfg.add[LOG_REGS+2-1:2] == 0)

...

case (running_state)
    idle : begin
        if (running_context == pointer_context && regfile_flags.full_context == 0) begin
            running_state <= idle;
            flags_o.start      <= 0;
            flags_o.is_working    <= 0;
        end
        else begin
            running_state <= starting;
            flags_o.start      <= 0;
            flags_o.is_working    <= 1;
        end
    end
    starting : begin
        // just to separate idle and running by an additional cycle
        running_state <= running;
        flags_o.start      <= 1;
        flags_o.is_working    <= 1;
    end
    ...
```

Then, the signal `flags_o.start` is sent to the module `mac_fsm`

```verilog
// file: mac_fsm.sv
...
case(curr_state)
    FSM_IDLE: begin
        // wait for a start signal
        ctrl_ucode_o.clear = '1;
        if(flags_slave_i.start) begin
            next_state = FSM_START;
        end
    end
    FSM_START: begin
        // update the indeces, then load the first feature
        ...
```

## Finish computation in Software and Hardware: two events

After HWPE finishes the computation, HWPE will send two events signals to the CPU.

```verilog
// file: hwpe_ctrl_slave.sv

begin
  for(int i=0; i<N_CORES; i++)
      flags_o.evt[i][N_EVT-1:1] <= (offloading_core[running_context] == i) ? ctrl_i.evt : '0;
  for(int i=0; i<N_CORES; i++)
      flags_o.evt[i][0] <= (offloading_core[running_context] == i) ? regfile_flags.true_done : 1'b0;
end
```

Then these events are reorganized at module `soc_peripherals` and then sent to the CPU:

```verilog
// file: soc_peripherals.sv

assign s_events...
assign s_events[140]              = fc_hwpe_events_i[0];
assign s_events[141]              = fc_hwpe_events_i[1];
assign s_events[159:142]          = '0;
```

We can find the actually, only $\frac{142}{160}$ events are used, hence, we can add more events here.

The indexes are defined in the software at `properties.h`:

```c
// file: properties.h

#define ARCHI_SOC_EVENT_FCHWPE0 140
#define ARCHI_SOC_EVENT_FCHWPE1 141
```

And there are another two software functions related to this events:

```c
// file: hwme.c

soc_eu_fcEventMask_setEvent(ARCHI_SOC_EVENT_FCHWPE0);
__rt_periph_wait_event(ARCHI_SOC_EVENT_FCHWPE0, 1);
```

which are defined/implemented at `soc_eu_v2.h`:

```c
// file: soc_eu_v2.h
static inline void soc_eu_fcEventMask_setEvent(int evt) {
  soc_eu_eventMask_setEvent(evt, SOC_FC_FIRST_MASK);
}
```

And `periph-v3.c`:

```c
// file: periph-v3.c

void __rt_periph_wait_event(int event, int clear)
{
  int irq = rt_irq_disable();

  int index = event >> 5;
  event &= 0x1f;

  while(!((__rt_socevents_status[index] >> event) & 1))
  {
    rt_wait_for_interrupt();
    rt_irq_enable();
    rt_irq_disable();
  }

  if (clear) __rt_socevents_status[index] &= ~(1<<event);

  rt_irq_restore(irq);
}
```


[^1]: https://hwpe-doc.readthedocs.io/en/latest/protocols.html