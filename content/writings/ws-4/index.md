---
title: "DevOps Platform Engineering for GitHub"
date: 2023-11-24
tags: ["standup", "platform engineering"]
---
{{< lead >}}
Welcome to my standups - a series where I share what I'm working on, thoughts rummaging around in my head, and what I'm excited for! 🚀 
{{< /lead >}}

## What I'm working on

When defining the platform for GitHub Actions, it was first important to differentiate the environment setup vs. the developer platform setup. 

The GitHub organization and teams strategy can differ greatly between companies. You may choose to have one large ORG under github with several TEAMS. Others may choose to have multiple ORGS depending on how large the company is.

The main point is this - when it comes to developer experience, it matters very little. The real crux of this setup falls on how locked-down you want the experience to be for teams to onboard to Github and administer their respective teams. 

<!-- insert photo of multi org structure -->
{{< figure
    src="images/orgs.png"
    alt="Example Multi-Org Structure"
    caption="Example multi-org structure in GitHub"
    >}}

When it comes to the developer platforming side, the boxes highlighted in yellow is where the main effort should go into. This is where we can really make a difference to the developer experience and help teams deploy their code quickly, safely, and as often as possible. 

If you come from an organization that mandates segregation of duties, you can look into having a dev repo (highlighted in blue) and an ops repo (highlighted in green). The reasoning behind this is due to the permissioning structure of GitHub and it’s repos. If you are a maintainer - you can remove or bypass any branch protection rules or environment rules that are in place, effectively negating their use in the first place. It also never looks good if both Dev and Ops have access to the same workflows and repos that deploy to both dev and production (especially with Audit). Take the safer route and split up the workflows, secrets, and configs used between each env in its own repo. 

{{< figure
    src="featured.png"
    alt="example structure of GitHub Platform"
    caption="Example structure of GitHub Platform"
    >}}

In the coming weeks, I’ll be mapping out a structure of workflows and rules to mandate in a company that's using GitHub for its CI/CD process.


### Repos for the week

{{< github repo="imranalley/go-track" >}}
* Added solution for [`resistor-color`](https://exercism.org/tracks/go/exercises/resistor-color)

## Musings

* What's actually better for a build pipeline - defining the environment at runtime or creating a defined image prior to the build
    * I know it can be a trade-off scenario and depands on multiple factors but we recently had this discussion at work that got me thinking about this

