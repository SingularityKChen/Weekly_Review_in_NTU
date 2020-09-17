---
layout: post
title: "[Glean] Impact Map"
description: "The introduction of impact map and how to create an impact map."
categories: [Glean]
tags: [Agile, Impact Map]
last_updated: 2020-09-17 16:37:00 GMT+8
excerpt: "The introduction of impact map and how to create an impact map."
redirect_from:
  - /2020/09/17/
---

* Kramdown table of contents
{:toc .toc}
# Impact Map[^1]

## What is an impact map

It is a mind-map grown during a discussion facilitated by answering the following four questions:

1. Why?
2.  Who?
3. How?
4.  What?

### Why

Why are we doing this? This is the goal we are trying to achieve.

#### Getting it right

Good goals tend to be ==**SMART: Specific, Measurable, Action-oriented, Realistic and Timely**==.

Goals should not be about building products or delivering project scope. They should explain **why such a thing would be useful**.

Goals should **present the problem** to be solved, not the solution. Avoid design constraints in a goal definition.

#### Examples

+ Starting to trade in Brazil by March next year
+ Increasing user conversion by 20% in three months

### Who

Who can produce the desired effect? Who can obstruct it? Who are the consumers or users of our product? Who will be impacted by it? These are the actors who can influence the outcome.

#### Getting it right

**Three types of actors**:

1. Primary actors, whose goals are fulfilled, for example players of a gaming system
2. Secondary actors, who provide services, for example the fraud prevention team
3. Off-stage actors, who have an interest in the behaviours, but are not directly benefiting or providing a service, for example regulators or senior decision-makers

Impact maps make us think about all these decision-makers, user groups and customer segments. By mapping out different actors, we can prioritise work better.

#### Examples

+ Mike Smith from the marketing department
+ Under-18 users with a mobile device at a concert
+ Apple ITunes store approvers

### How

It answers the following questions: How should our actors' behaviour change? How can they help us to achieve the goal? How can they obstruct or prevent us from succeeding? These are the impacts that we're trying to create.

#### Getting it right

Don't list everything an actor might want to achieve. List only the impacts that really help move you in the right direction towards the central goal.

Ideally show a change in actor behaviour, not just the behaviour. Show how the activity is different from what is currently possible.

Consider negative or hindering impacts as well as positive ones.

#### Examples

+ Inviting more friends
+ Purchasing tickets without calling the call centre
+ Selling tickets faster

### What

The third level of an impact map answers the following question: What can we do, as an organisation or a delivery team, to support the required impacts? These are the deliverables , software features and organisational activities.

#### Getting it right

This is the least important level of an impact map. Don't try to make it complete from the start. Refine it iteratively as you deliver.

Don't go into a lot of detail early on, there will be time for that later. List only high-level deliverables. You can break down high-level features into lower-level scope items, such as user stories, spine stories, basic or extension use cases later.

#### Examples

+ On-line ticket sales
+ Mobile home page with purchase form
+ Optimise call centre sales scripts
+ Sign re-selling contracts

**Never aim to implement the whole map.**

**Instead, find the shortest path through the map to the goal!**

## Creating an impact map

### Preparation

#### Discover real goals

If you can't find any objectives, start with suggested features and ask whose behaviour will this change and in what way. Then ask why those behaviour changes are important. Alternatively, ‘why is the feature important’ or ‘how would it be useful’.

#### Define good measurements

I recently facilitated a workshop where the initial list had roughly 20 goals. By **discussing measurements and targets**, we dropped 17 of those 20 goals – they weren't that important.

#### Plan your first milestone

Once you have a list of goals and measurements, scrutinise it. Check if everything is really part of the current milestone or if you can postpone some items.

### Mapping

#### Draw the map skeleton

Put the first milestone in the centre of the map, and connect some key high-level deliverables to it.

Make sure to define precisely the actors and impacts, and double-check that the connections still makes sense:

+ Is it realistic that the feature will contribute to the impact?
+ Is the impact valid for the actor?
+ Will the impact really contribute to achieve the goal?

It's a good idea to split a large group into smaller groups and merge after 20 minutes, comparing the results. This will get you to a good skeleton sooner.

#### Find alternatives

Try to define as many alternatives as you can in a time-boxed period, limiting the discussion to the actors and behaviour impacts. This corresponds to the divergent phase of design thinking. Diverge and merge. Get groups to work independently and review results every 20 minutes. The goal of this step is to try to find a better or a cheaper solution, a shorter journey to the key objectives. Don't criticise any ideas, just throw them on there. Use the existing skeleton map structure as an inspiration and ask the following questions:

+ What else could those guys do for us?
+ Who else can help? How?
+ Who can obstruct us?

#### Identify key priorities

phase. With plenty of options, we need to make some choices. Start by asking the following questions:

+ Look at possible obstructions: what are the crucial things that can stop us before we even start?
+ Is there a high-value low-hanging-fruit impact somewhere?
+ What are the key assumptions to test?

#### Earn or learn

Having identified the actors and impacts, we can start growing the third level of the map: deliverables.

Ask the following questions to start a useful discussion on deliverables:

+ What is the simplest way to support this activity? What else could we do?
+ If we're unsure about the assumption, what is the simplest way to test it?
+ Could we test it without software?
+ Could we start earning with a partly manual process?

[^1]: Impact Mapping: Making a Big Impact with Software Products and Projects
