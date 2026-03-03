---
title: "Schedule Query in BigQuery"
date: 2026-03-02
draft: false
tags: ["Big Query", "Automation", "Apps Script","Data Engineering","JavaScript"]
---

I wanted to share a quick look into why and how I set up a weekly snapshot query in BigQuery in my daily work. It’s a simple task, but the mindset behind it is fundamental to data reliability.

## The Background: Why History Matters

In a live production database, data is "fluid." A customer might change their representative (rep) today, and the old value is gone forever in the old system.

If a stakeholder asks, "Who was the sales rep for Customer A three months ago?" and you haven’t been tracking changes, you’re stuck. That’s why we build Snapshots.

## What is a Snapshot?

Think of a snapshot as a "photo" of your data. Instead of just seeing the current state, we append the data into a history table every week.

**The Goal:** To transform a "Current State" table into a "Historical Trace" table.

**The Benefit:** It allows us to track trends, churn, and movements over time without needing complex Logic like SCD (Slowly Changing Dimensions) Type 2 right away.

## The Implementation

I use a simple INSERT INTO statement. The key here is the snapshot_date. By hardcoding the date of the "photo," we can filter our history later.

```sql
INSERT INTO Project.dummy_data.customer_trace (dc_id, cust_code, rep, snapshot_date)
SELECT
    dc_id,
    cust_code,
    rep,
    CURRENT_DATE(‘Australia/Sydney’)
FROM Project.sales.customer_dc
```

## 3 Simple Steps to Set It Up

If you’re doing this in BigQuery, you don’t need a heavy orchestrator like Airflow for simple tasks. Here is the workflow:

**Create the Target Table:** Make sure your customer_trace table has the exact same schema as your source, plus one extra column: snapshot_date.

**Test the Query:** Run the SELECT portion first to ensure your timezones (like Australia/Sydney) are capturing the correct "now."

**Schedule in BigQuery:**
- Paste the code into the BigQuery Editor.
- Click Schedule > Create new scheduled query.
- Set the repeat frequency to Weekly (e.g., every Monday at 1 AM).

## Summary

By setting this up, we stop losing data history. 