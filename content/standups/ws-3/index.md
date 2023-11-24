---
title: "WS-3: The CIA Triad"
date: 2023-11-17
series: ["Weekly Standups"]
series_order: 3
---
{{< lead >}}
Welcome to my weekly standups - a series where I share what I'm working on, thoughts rummaging around in my head, and what I'm excited for next week! 🚀 
{{< /lead >}}

## What I'm working on

It was a long weekend this week and the main thing thats been on my mind was JBBQ - tried it for the first time with co-workers and sad to say it derailed the rest of my week. Don’t make the same mistakes I made and make sure to drink plenty of water when taking in that much salt in one sitting :’D.

An interesting bug I found out about this week was when I was testing the cloud pod templates we administer for the enterprise. When upgrading to `jdk11` from `jdk8`, we decided to keep version 8 as a backup for those teams that weren’t ready for the switch. The problem lies in the call you make to install `jdk8` within your image. The go to command is `sudo yum install java-1.8.0-openjdk` where you think that this command is installing the jdk - when in fact, it’s installing the JRE. When you `ls -la /usr/lib/jvm/` you can see the symbolic links that eventually point to the jre. The solution? Simply adding `-devel` to the command: `sudo yum install java-1.8.0-openjdk-devel`

### Repos for the week

{{< github repo="imranalley/go-track" >}}
* progress was stalled for this week - aiming to get back on track with more intermediate exercises

## Musings

The CIA Triad refers to the tradeoffs you make between Confidentiality, Integrity, and Availability within security systems. With our current project of defining the platform for GitHub Enterprise Cloud, we have to figure out how to meet these segments within our processes. When you’re working with one platform to deploy to both dev and prod, coming up with segregation of duties, best practices, and secrets management becomes paramount to the success of the dev platform. I’m slowly starting to see my role as more of a platform engineering role - helping define the pipeline templates and guidelines for other teams across the enterprise to leverage for their deployment needs.  


In other news:
* Looks like creators aren’t liking the new m3 pro laptops that apple has released
* https://youtu.be/Fv78mPxRp9A?si=QMtHr-3RwWQmTPS-
* https://youtu.be/XlX-GjICr6w?si=XbaAgVhK9Q8NDJGr 

## Things I'm excited for next week

* Engaging with a team for kickstarting their deployment process with Ant

