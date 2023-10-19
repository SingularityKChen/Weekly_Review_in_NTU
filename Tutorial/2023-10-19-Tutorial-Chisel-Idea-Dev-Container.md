---
layout: post
title: "[Tutorial] Develop Chisel with Dev Container in Idea."
description: "This article introduces how to develop Chisel with Dev Container in IntelliJ Idea."
categories: [Tutorial]
tags: [Chisel, Idea, Docker]
last_updated: 2023-10-19 15:40:00 GMT+8
excerpt: "This article introduces how to develop Chisel with Dev Container in IntelliJ Idea."
redirect_from:
  - /2023/10/19/
---

* Kramdown table of contents
{:toc .toc}

# Why Dev Container?

The Dev Container is a lightweight, self-contained environment that allows you to develop in a containerized environment.
It is a good choice for developing Chisel, because it can provide a consistent development environment for all developers.

# Why IntelliJ Idea?

IntelliJ Idea is a powerful IDE for Scala development.
It supports Chisel highlighting and definition navigation well.

# How to use Dev Container in Idea?

Here we use the [chisel-chipware](https://github.com/SingularityKChen/chisel-chipware/tree/main) project as an example.

## Step 1: Enable Dev Container Plugin

From the [official document](https://www.jetbrains.com/help/idea/connect-to-devcontainer.html), we need to enable the `Dev Containers` plugin first.

Then open the `chisel-chipware` project in Idea, open the `.devcontainer/devcontainer.json` file, and click the `Create Dev Container and Mount` button.

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/devcontainer_json_idea.png" alt="Create Dev Container and Mount" style="zoom:50%;" />

Then wait for the Dev Container to be built.

## Step 2: Open the Project in Dev Container and generate bsp

After the Dev Container is built, we can open the project in the Dev Container.

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/devcontainer_build.png" alt="Build Dev Container" style="zoom:50%;" />

We need to generate the bsp file first.
Execute the following command in the terminal:

```bash
mill -i -j 0 mill.bsp.BSP/install
```

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/devcontainer_gen_bsp.png" alt="Generate bsp" style="zoom:50%;" />

Then we open any `.scala` file and install the Scala plugin.
Then we restart the Idea.

## Step 3: Open the Project in Dev Container again and import bsp

After the bsp file is generated, we can open the project in the Dev Container again.
Then we can import the bsp file for Chisel development.

You may either [search for the **Project from Existing Sources** action or select **File | New | Project (Module) from Existing Sources**.](https://www.jetbrains.com/help/idea/bsp-support.html#import_bsp)

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/devcontainer_import.png" alt="Select file" style="zoom:50%;" />

Then we can select the bsp file to import.

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/devcontainer_import_bsp.png" alt="Import module from bsp" style="zoom:50%;" />

After the import is complete, we can develop Chisel in the Dev Container and navigate the definitions.