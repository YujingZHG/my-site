---
title: "Automating Weekly Reports with Apps Script"
date: 2025-02-12
draft: false
tags: ["Big Query", "Automation", "Apps Script","Data Engineering","JavaScript"]
---

## The Problem

Traditional weekly reporting process was simple but tedious: every Monday morning, someone had to manually query BigQuery, copy results into a spreadsheet, format the data, and send it via email. It took about 20 minutes each week and was easy to forget or delay.

As engineers, we knew this needed automation.

## Solutions:
we can use several solutions depend on our infrastructions.

1.Python Script

2.Cloud Script

In this log I'm going to introduce the cloud script one. My work is based on a cloud database and Apps Script. I chose this methods is based on our big query database which is easy to connected and triggered by apps script and gmail.

Other benefits: 
**Zero Infrastructure Overhead**  
No need to manage service accounts, configure VPCs, or deal with deployment pipelines. Apps Script runs in Google's infrastructure—you just write code and hit save.

**Built-in Integration**  
BigQuery, Gmail, and Google Sheets work together seamlessly. The APIs are simple and well-documented. No authentication headaches or SDK version conflicts.

**Native Scheduling**  
The trigger system is straightforward: point-and-click to set "every Monday at 8 AM." No cron syntax, no separate scheduling service.


## Implementation Overview

The entire solution consists of three parts:

1.Apps Script connection.

2.Test and Format.

3.Send Email.


The actual code is very simple and the function is already in GAS just follow the format to call the function. The BigQuery API returns results directly and use Gmail sends it. No external libraries, no dependencies.

## Key Learnings

**It's Faster Than You Think**  
POC took about an hour. Most of that time was spent formatting the email HTML, not wrestling with infrastructure.

**Data Structure Quirk**  
BigQuery results in Apps Script use a nested structure rather than flat JSON. Once you understand this pattern, it's straightforward to work with.

**Less handle part less debug**  
Compare to python script the setting is easy and less likly to debug every where because the connection and send email part is already fixed.(less flexibility with less efforts for sure)

## Why This Approach Works

Apps Script shines for lightweight automation within the Google ecosystem. It's not the most powerful tool, but for the specific use case of "query BigQuery → send formatted email," it's the most efficient path from zero to production. Especially when you already use google's product in your work.

The key insight: **not every automation needs complex infrastructure**. Sometimes the simplest solution is the right one.

**Summary**

Automation the regular reports can definetly save your time.

- **Setup time:** ~1 hour  
- **Weekly time saved:** 20 minutes

For teams already using Google Workspace and BigQuery, Apps Script is a no-brainer for this type of recurring report automation.
