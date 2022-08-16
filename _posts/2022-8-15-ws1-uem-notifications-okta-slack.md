---
layout: post
title: Send Slack Notifications Using Okta Workflows When Devices Enroll / Are Wiped From Workspace One UEM
categories: updates
comments: true
---

In a previous job I referenced [TaniaComputer's excellent blog post](https://www.taniacomputer.com/?p=58) for Jamf computers reporting when a computer was successfully enrolled via Slack notification to a dedicated channel. This was valuable to us and helped on a few occassions with troubleshooting enrollments and other issues. 

At my current job, I wanted to recreate this experience for the Windows platform using WorkspaceOne UEM and Slack to send these notifications. This is the story of my journey. 

## WorkspaceOne UEM
In WorkspaceOne UEM there is a Notifications API buried in 
**Groups & Settings > All Settings > System > Advanced > API > Event Notifications**.
For **Target Name** enter a name like "Endpoint Enroll/Wipe Notifications".
For **Target URL** go to [https://webhook.site](https://webhook.site) and generate a **temporary** webhook to see the data being sent on event trigger.
Under **Events** mark the checkboxes for the following **Device Enrollment**, **Device Unenrolled**, **Enterprise Wipe**, **Device Wipe**, **Device Delete**.
Click on **Test Connection** you should see a success.
![](/images/notification-events.png)
Go to your [https://webhook.site](https://webhook.site) and examine the json output. It should look similar to this:
![](/images/sample-json.png)
We see lots of interesting information we can pull into a Slack notifications channel.

## Slack - Build a Bot
I wont go into detail here except say that you need to build a slackbot and setup a webhook to send notifications to a slack channel. The method is straightforward and can be accessed [here](https://slack.com/help/articles/115005265063-Incoming-webhooks-for-Slack).

## Okta Workflows - Putting it all together
In **Okta Admin** launch **Okta Workflows Console** create a new **API Endpoint** card and under **Security Level** select **Expose as Webhook**
Leave the rest of the values at default, then close and save your workflow. This will generate the correct **Invoke URL**. 

Open back up the **API Endpoint** card and copy the **Invoke URL** address.

Go back to **WorkspaceOne** and replace the [https://webhook.site](https://webhook.site) URL with the one you just copied from **Okta Workflows**

Go back to **Okta Workflows** and close the **API Endpoint** card. 
Add a function **Object - Get Multiple** and drag the body from the **API Endpoint** card to the **Object - Get Multiple** body to map it. 
Add the keys you want to capture from the json output earlier. Ensure they are named **exactly** as they appear on the json output. 
![](/images/workflow-step-1.png)

The next step was made by using [Block Kit Builder](https://slack.com/events/getting-started-with-block-kit-for-the-non-developer). Use this to construct the message however you like. This is just a guide for my initial config. 
Create a **Text Compose** card and drag the relevant bits of information from the **Object Get Multiple** over to the text compose card. 
![](/images/workflows-step-2.png)

Create a new **API Connector Post** card and drag the **Output** of the **Text Compose** card to the **Body** of the **API Connector Post** card.
![](/images/workflows-step-3.png)

In the **API Connector Post** card in the **URL** field put your **Slack Webhook** from your Slack Bot you created earlier.

Save and run the flow. You should start seeing outputs like this in your Slack Channel:
![](/images/working-bot-example.png)

**Text Compose** card text minus the variables below:

{% gist 7ec7a99d05e69318f9a1f817bb7c1f8f %}

<script type='text/javascript' src='https://storage.ko-fi.com/cdn/widget/Widget_2.js'></script><script type='text/javascript'>kofiwidget2.init('Support Me on Ko-fi', '#29abe0', 'D1D3EFTES');kofiwidget2.draw();</script> 