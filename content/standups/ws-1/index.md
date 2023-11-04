---
title: "Scaling DevOps Across an Enterprise"
date: 2023-11-03
series: ["Weekly Standups"]
series_order: 1
---
## What I'm working on

Everyone's favourite topic, audit. I'm preparing scripts that are self serve for teams across an enterprise to scan their Jenkins controller's and return all the plugin's they are currently using with the version numbers. We're preparing for a major upgrade that'll remove the support of dockershim based on the [latest updates from kubernetes](https://kubernetes.io/blog/2022/02/17/dockershim-faq/). Upgrades are always very procedural and you have to take things step by step, which I really like. The only thing you have to watch out for is to mitigate the unknown issues that may arise from these major upgrades (hence the audit). We're combing through the enterprise pod templates and making sure that the newly upgraded platform can support the existing pods we have or if we may need to upgrade these images as well.

### Repos for the week

{{< github repo="imranalley/go-track" >}}
* Continuing my progress to learn golang

## Musings

Burning in the background is an enterprise strategy to shift our main DevOps CI/CD activities to GitHub Actions. It's always interesting to plan out how to set up this enterprise offering so people can easily adopt to the platform and the controls you want to have in place to mitigate risk. Questions that come to mind:
* How should we set up the organization/teams structure
* Should we lockdown secrets for the enterprise to push people to use the Enterprise approved tools (Vault/CyberArk)
* Runners compute and scaling solution
* How should we support open-source actions within the enterprise
* Adoption/migration strategy from Jenkins to GHA

## Things I'm excited for next week

* Building out self-serve actions for the enterprise to audit their Jenkins plugins
* Blasting our pods and testing them to smithereens for our upgrade 