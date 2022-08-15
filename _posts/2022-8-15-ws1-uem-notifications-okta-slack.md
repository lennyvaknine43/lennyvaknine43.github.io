---
layout: post
title: Send Slack Notifications Using Okta Workflows When Devices Enroll or are Wiped From Workspace One UEM
categories: updates
comments: true
---

In a previous job I referenced [TaniaComputer's excellent blog post](https://www.taniacomputer.com/?p=58) for Jamf computers reporting when a computer was successfully enrolled via Slack notification to a dedicated channel. This was valuable to us and helped on a few occassions with troubleshooting enrollments and other issues. 
At my current job, I wanted to recreate this experience for the Windows platform using WorkspaceOne UEM and Slack to send these notifications. This is the story of my journey. 

## WorkspaceOne UEM
In WorkspaceOne UEM there is a Notifications API buried in 
**Groups & Settings > All Settings > System > Advanced > API > Event Notifications**
For **Target Name** enter a name like "Endpoint Enroll/Wipe Notifications"
For **Target URL** go to [https://webhook.site](https://webhook.site) and generate a **temporary** webhook to see the data being sent on event trigger.
Under **Events** select **Device Enrollment** **Device Unenrolled Enterprise Wipe** **Device Wipe** **Device Delete**
Click on **Test Connection** you should see a success.
![](/images/notification-events.png)
Go to your [](https://webhook.site) and examine the json output. It should look similar to this:
![](/images/sample-json.png)
We see lots of interesting information we can pull into a slack notifications channel.

## Slack - Build a Bot
I wont go into detail here except say that you need to build a slackbot and setup a webhook to send to a slack channel. More info soon [placeholder for how to slackbot + webhook]

## Okta Workflows - Putting it all together
In **Okta Admin** launch **Okta Workflows Console** create a new **API Endpoint** card and under **Security Level** select **Expose as Webhook**
Leave the rest of the values at default, then close and save your workflow. This will generate the correct **Invoke URL**. Open back up the **API Endpoint** card and copy the **Invoke URL** address.
Go back to **WorkspaceOne** and replace the [https://webhook.site](https://webhook.site) URL with the one you just copied from **Okta Workflows**
Go back to **Okta Workflows** and close the **API Endpoint** card. 
Add a function **Object - Get Multiple** and drag the body from the **API Endpoint** card to the **Object - Get Multiple** body to map it. 
Add the keys you want to capture from the json output earlier. Ensure they are named **exactly** as they appear on the json output. 
[todo] add text compose + api post slack connector. 
more soon
