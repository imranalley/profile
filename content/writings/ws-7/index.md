---
title: "Scaling GitHub Enterprise Cloud - Key Learnings from A Platform Engineering Perspective"
date: 2025-07-29
description: ""
summary: ""
tags: ["platform engineering", "github", "ansible", "AKS", "Python"]
---

Standing up GitHub Enterprise Cloud (GHEC) across a large organization — especially in regulated environments — comes with both opportunities and engineering challenges. Here’s what I learned during our journey 🚀


## Providing Context Across the SDLC with Custom GitHub Apps and Properties
We needed a way to enrich the software delivery pipeline with contextual metadata—things like security scan status, change approvals, and ticket tracking. While you can leverage the built in Custom Properties feature in GitHub - we needed a more robust solution via GitHub Apps

We built a GitHub App that listens to webhook events and attaches custom properties to commits and pull requests. This is then fed to a source of truth application (for most organizations, this would be ServiceNow or an equivalent tool) that now becomes "context-aware" of whats actually going on in the code base and where the code at this specific commit hash is on it's way to production. 

{{< highlight json "linenos=table" >}}
{
  "appsecScanPassed": true,
  "serviceNowTicket": "INC123456",
  "changeRecord": "CHG987654",
  "repoTier": "Tier 1",
  "gitHash": "abc123"
}
{{< /highlight >}}
✅ Example: Pull Request Check Context

## Scaling with Self-Hosted Compute (VMs + AKS)
As workloads scaled, we needed to offload from GitHub-hosted runners to self-hosted compute using a mix of Linux VMs and Azure Kubernetes Service (AKS).

🧱 VM Runner Setup (via Ansible)
Teams could onboard their own VMs using a custom Ansible playbook that we wrote and dynamic inventories containing the info of their hosts:

{{< highlight yml "linenos=table" >}}
- name: Install GitHub Runner
  hosts: github_runners
  become: yes
  roles:
    - install_docker
    - setup_github_runner
{{< /highlight >}}

To support large-scale CI/CD workloads without relying solely on self-hosted runners on standalone VMs, we deployed self-hosted compute using Azure Kubernetes Service (AKS). To make this scalable and dynamic, we leveraged [Actions Runner Controller (ARC)](https://github.com/actions/actions-runner-controller)—an open-source project that enables GitHub Actions runners to be orchestrated as Kubernetes pods.

{{< github repo="actions/actions-runner-controller" showThumbnail=true >}}

⚙️ On-Demand Runner Provisioning with ARC allowed us to:
* Automatically spin up runner pods based on GitHub job demand
* Integrate with GitHub at the org or repo level
* Enforce node-level security, autoscaling, and resource isolation

### The Cross-Cloud Caching Challenge
While ARC gave us elasticity, we hit a major bottleneck with caching. Our GitHub runners in AKS needed access to artifacts (Docker images, language binaries, build caches), but our Artifactory lived in a different cloud provider—outside the AKS VNet.

This meant:
* No shared VPC or peering → slower download speeds
* No native layer caching → redundant traffic, higher egress costs
* Inconsistent build times → degraded developer experience

🛠️ Workaround: Localized Persistent Caching
To reduce these inefficiencies, we mounted a PersistentVolume to runner pods as a local cache for build artifacts and Docker layers depending on use-cases on a per app basis. Not the Best solution - I know - but this would give us time while we figure out our hosting solution for internal tooling and hopefully migrate our binary storage to Azure to take advantage of the backbone network that would significantly reduce wait times when downloading artifacts.

🔍 The key takeaway? ARC helped us scale runners elastically, but caching strategies are critical — especially when your CI/CD pipeline spans cloud boundaries. For teams in hybrid or multi-cloud environments, proximity to artifacts and intelligent caching layers can make or break developer throughput and experience.

## Automating Platform Ops with Python + Ansible
### 📦 Repo Migration + Metadata Collection
We built a Python service that integrates with GitHub and Jira to:

1. Enrich Custome Properties with team ownership, tier, compliance data, and any other context properties required
2. Automate Runner Group creation and assignments to confidential repos.
3. Automate migration from GitHub Enterprise Server to Cloud

## 🧩 Final Thoughts
The GHEC platform becomes truly enterprise-grade only when paired with:

✅ Context-aware delivery pipelines
⚙️ Scalable, flexible compute
🚀 Seamless automation

These three pillars helped us build a platform that balances security, auditability, and developer velocity.
