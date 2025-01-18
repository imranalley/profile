---
title: "Legacy Apps and DevOps"
date: 2023-12-01
tags: ["standup", "platform engineering"]
---
{{< lead >}}
Welcome to my standups - a series where I share what I'm working on, thoughts rummaging around in my head, and what I'm excited for! 🚀 
{{< /lead >}}

## What I'm working on

‘Legacy’ apps can differ in meaning across various organizations. For a current project, I was tasked with “DevOpsifying” the build and deploy process for an ant application. It’s considered a legacy app solely due to the fact that it hasn't migrated over to a more robust building tool like Maven or Gradle. The engaging team had a simple request: Help us build our application in the cloud using jenkins and publish the artifacts in a central location to later be deployed to our environments. Here’s what I learned:

### Dependency Management

This particular ant application was referencing many local jar files. Paths were also hardcoded, and these dependencies weren’t being pulled from a central repository like `maven-central`. Herein lies the first problem - helping the dev team understand that building in the cloud means you have to change your approach with how you structure your application and `build.xml` file.


<!-- insert photo of local env setup -->
{{< figure
    src="images/local.png"
    alt="Local Build Setup"
    caption="Local Build Setup"
    >}}

First we have to reformat the `build.xml` file to reference relative paths in the source code or pre-built docker image that you use as a build agent. You also have to start storing the jars that are referenced in the app in a central binary repository like Artifactory. Doing it this way helps you identify which item in the build process has changed, whether it’s a dependency or source code. 

{{< figure
    src="images/cloud.png"
    alt="Cloud Build Setup"
    caption="Cloud Build Setup"
    >}}

If the dev team still wishes to have the ability to build locally, you also have to warn the team about potential drift that may occur if they start to change things locally and not reflect these same changes in the build process for the cloud.

Next week, I’ll cover the deployment process for the legacy team. 

## Musings

* December is upon us - perhaps this is the year I try out advent of code.

