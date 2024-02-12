---
title: "Creating an NBA Fantasy Scheduler using Chat GPT"
date: 2024-02-04
description: " "
summary: " "
tags: ["side projects", "Chat GPT"]

---

Hello! It’s been a while!

It’s been a nice break over the holidays and into the new year - but it feels nice to be back into the swing of things and writing again. 

I spent some time over a weekend working on an automated NBA scheduler that will scrape hashtag basketball https://hashtagbasketball.com/advanced-nba-schedule-grid and pull down the week's schedule in an automated fashion every monday using python. 

Chat GPT was fairly successful in creating the base script for me! 

The original ask was simple enough: 

Write me a python script that uses beautiful soup to scrape the nba schedule grid located here: https://hashtagbasketball.com/advanced-nba-schedule-grid. Output the formatting in a markdown file.

I would then use ghost.io to help publish the schedule and have my friends and I consume it on a weekly basis. Seemed easy enough.

The problem was that ghost.io (and many other email list tools) was on a trial, after which I would have to pay $9 to use their service each month. I was definitely not down for that. 

Enter https://sendgrid.com/en-us - an email delivery solution that manages your own SMTP server to send emails from. The best part was that popular email solutions like google’s gmail wouldnt block these emails coming in as spam as opposed to hosting your own SMTP server and having to create certificates/verification processes. 

My Automation orchestration tool of choice was Jenkins - fairly out of date for some but the ability to run Jenkins on your own machine in docker is a great way to get automation going for a simple project like this. I pulled down the LTS jenkins image and configured sendgrid’s SMPT information to get started with sending emails.

The GPT request changed to accommodate the fact that email bodies in Jenkins only support HTML templates as a body replacement and not markdown. Gippty (my way of saying GPT) handled this very easily.

{{< figure
    src="images/email.png"
    alt="Example email that gets sent out every week"
    caption="Example email that gets sent out every week"
    >}}

I created a simple Jenkinsfile that would take in the output of my python script (nba-schedule.html) and have it emailed to my friends and I to use in our fantasy league preparations. 

{{< highlight groovy "linenos=table,hl_lines=7" >}}
stage('Send Email') {
    steps {
        script {
            // Read the generated HTML file
            def htmlContent = readFile('nba_schedule.html')
            // Send email using Email Extension plugin
            emailext body: htmlContent,
                      mimeType: 'text/html',
                      subject: 'NBA Schedule Automation W16: 5 Feb - 11 Feb',
                      to: 'bcc:{email here}@gmail.com'
                      attachLog: false
        }
    }
}

{{< /highlight >}}

And voila - within a span of a weekend, I (along with Gippty) had hacked together a simple automated solution that would let my friends be more competitive in our fantasy basketball league - as if we weren’t competitive enough already. 
