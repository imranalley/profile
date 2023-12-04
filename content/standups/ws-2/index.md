---
title: "Laying the Groundwork"
date: 2023-11-10
series: ["Weekly Standups"]
series_order: 2
description: "Weekly Standup #2"
summary: "Weekly Standup #2"
tags: ["weekly standup", "playform engineering"]
---
{{< lead >}}
Welcome to my weekly standups - a series where I share what I'm working on, thoughts rummaging around in my head, and what I'm excited for next week! 🚀 
{{< /lead >}}

## What I'm working on

Last week, I was writing up scripts that would let GitHub Enterprise Site Admins audit entire orgs within their enterprise servers. I used my [open source GitHub action](https://github.com/imranalley/enterprise-audit-action) as a base layer and built on top of that. I intend to use my learnings here and update it with the new feature!

There also was a need to quickly make a script that would go through all CJE Jenkins controllers and the plugin's that they have installed on them in preparation for a major upgrade. You can expect this being up on GitHub soon as well!

### Repos for the week

{{< github repo="imranalley/go-track" >}}
* Continuing my progress to learn golang
    * added solutions for `gigasecond`, `isogram`, `leap` on [exercism](https://exercism.org/tracks/go/exercises)

## Musings

The current solution for enteprise pipeline templates lies with Jenkin's Share pipeline libraries feature. How would this translate to a GitHub Action Pipeline library? Is this really the solution we want to go for or can we just start creating workflows and actions and then post them up on our internal github marketplace catelogue for consumption? Clearly, GitHub has been on my mind a lot :D 

In other news:
* Are the M3 Macs really that good of an upgrade?


## Things I'm excited for next week

* Starting to build out workflows to deploy to OCP4