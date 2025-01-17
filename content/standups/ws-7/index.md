---
title: "Running GHA Runners in VMs"
date: 2025-01-17
series: ["Standups"]
series_order: 7
description: "A short bit on lessons learned from managing runners on a VM for an "
summary: " "
tags: ["standup", "platform engineering", "github actions"]
---
{{< lead >}}
Welcome to my standups - a series where I share what I'm working on, thoughts rummaging around in my head, and what I'm excited for! 🚀 
{{< /lead >}}

## What I'm working on

I’ve missed a week to recover from what I think was one of the worst bouts with the flu in my lifetime. Phew, glad that's over (almost - that darn post-nasal drip).

### GitHub and its many edge cases

#### Docker Actions
Creating docker actions in javascript can work for many organizations, but for teams who perhaps come from different backgrounds/experiences and who want to avoid technical debt (using a language that people on the team don’t know so well), they might go the docker action route. 

The setup for creating a python docker action is fairly straightforward:

<!-- insert photo of docker action setup -->
{{< figure
    src="images/action.png"
    alt="Docker Action Setup"
    caption="Python Docker Action Setup"
    >}}

You have your `action.yml`, `main.py` (and any other files/code needed for the action i.e `requirements.txt` etc.), and your `Dockerfile`.

Now when you’re in an enterprise environment, you may not like the fact that the Docker image registry defaults to public dockerhub. 
{{< highlight dockerfile "linenos=table,hl_lines=1" >}}
FROM python:3-slim # This gets pulled from public dockerhub
WORKDIR /app
COPY . /app
RUN pip install --target=/app -r requirements.txt
CMD ["python", "/app/main.py"]
{{< /highlight >}}
And even if you try to tag the image with your internal docker repository, GitHub doesn’t let you authenticate before trying to pull down the image when you call a docker action. 

The workaround?

{{< highlight yml "linenos=table" >}}
container:
  image: ghcr.io/owner/image
  credentials:
     username: ${{ secrets.USER }}
     password: ${{ secrets.TOKEN }}
{{< /highlight >}}

Declare the job to run in a container and then call a shared script from another github repo to complete your work. It’s a bit messy, but it works!


