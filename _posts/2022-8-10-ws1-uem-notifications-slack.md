---
layout: post
title: Send Slack Notifications When Devices Enroll or are Wiped From Workspace One UEM
categories: updates
comments: true
---

In a previous job I referenced [TaniaComputer's excellent blog post](https://www.taniacomputer.com/?p=58) for jamf computers reporting when a computer was successfully enrolled via slack notification to a dedicated channel. This was valuable to us and helped on a few occassions with troubleshooting enrollments and other issues. 
At my current job, I wanted to recreate this experience for the Windows platform using WorkspaceOne UEM and Slack to send these notifications. This is the story of my journey. 
In WorkspaceOne UEM there is a Notifications API buried in 
>>Groups & Settings > All Settings > System > Advanced > API > Event Notifications