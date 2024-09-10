# Understanding Backup Schedule and Retention Policy

<!--
Task: Write an Article Titled "Understanding Backup Schedule and Retention Policy"
The goal of this article is to clearly explain the relationship between a backup schedule and a backup retention policy. It should address the following key questions in a user-friendly manner:
How are backup schedule and backup retention policy related?
How does this relationship impact the amount of backup data stored?
How does this relationship affect data restoration options?
Additionally, provide a few examples of backup schedule and retention policy configurations, along with their impact on storage usage and available restore options.
-->

## Introduction

> * A well-planned backup schedule and retention policy are important for data management.
> * This article explains backup schedules, retention policies, and their relation.
> * Readers will learn how to choose the right backup schedule and retention policy for their objectives.
> * Readers will also be able to estimate the amount of storage required for backups and its cost.

## Defining Backup Schedule and Retention Policy

### Backup Schedules and Types

> * Backup is a copy of data from a point in time, securely stored and able to be restored for usage.
> * Different types of backups — full, incremental, and differential — have distinct roles in a balanced backup strategy.
> * Full backups: full copy, much longer to make, normal recovery time.
> * Incremental backups: what's changed since the last backup, very fast to make, and slightly longer to recover.
> * Differential backups: what's changed since the last full backup, slower to make and faster to recover than incremental.
> * A good backup schedule combines full and incremental/differential backups at points in time, aiming at some objective.

### Backup Retention Policy

> * The purpose of a retention policy is to ensure that backups are available when they're needed, thus giving reliability.
> * Retention policy dictates how long to keep backups, both full and incremental or differential.
> * There's an absolute minimum to store: the latest full backup plus what comes after it.
> * But it can provide insufficient reliability because data can get corrupted. Thus, it makes sense to store earlier backups.
> * "Naive" policy: store the latest N full backups.
> * FFI policy: short description and link.
> * GFS policy: short description and link.

## How to Tailor Backup Schedule and Retention for Data Restoration Objectives

> * Data restoration is the ultimate goal of backing up data.
> * Restore Time Objective and Restore Point Objective.
> * Backup frequency improves RTO and RPO at the cost of storage and system performance.
> * Retention adds reliability, again at the cost of storage.

## How to Estimate Amount of Data Stored and Storage Cost

> * Now that we know the optimal schedule and retention policy, we can calculate the storage amount.
> * Think about the data modification rate, desired objectives, and reliability.
> * If you think the cost is too high, compare it with risks for restoring to an earlier point in time or not restoring at all.
> * If it seems cheap, consider retaining backups for a longer time or increasing frequency.

## Example A: GFS strategy for Mission-Critical Data

## Example B: FFI strategy for Important Data

## What's Next to Learn

> * Mastering backups doesn't stop on backup schedules and retention.
> * Plan your restoration strategy, considering your objectives. Consider different strategies for each type of data.
> * Check how your backups are restored. Do it regularly
> * Automate the backing up and restoration workflows.

## Todo

- [ ] Links to other blog articles on related topics