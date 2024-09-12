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

<!--
> * A well-planned backup schedule and retention policy are important for data management.
> * This article explains backup schedules, retention policies, and their relation.
> * Readers will learn how to choose the right backup schedule and retention policy for their objectives.
> * Readers will also be able to estimate the amount of storage required for backups and its cost.
-->

A well-planned backup schedule and retention policy are important for effective data management.
Together, they build a foundation for protecting and restoring critical data,
fulfilling data restoration objectives, and allowing for predicting and planning backup costs.
This blog post explains what backup schedules and retention policies are, and how they relate to one another.

The goal of the post is to help you make an informed decision, assessing the risks and associated costs.
After reading it, you will be able to choose the right backup schedule and retention policy for your objectives.
You will also estimate the amount of storage required for the chosen configuration.

## Defining Backup Schedule and Retention Policy

### Backup Types and Schedules

<!--
> * Backup is a copy of data from a point in time, securely stored and able to be restored for usage.
> * Different types of backups — full, incremental, and differential — have distinct roles in a balanced backup strategy.
> * Full backups: full copy, much longer to make, normal recovery time.
> * Incremental backups: what's changed since the last backup, very fast to make, and slightly longer to recover.
> * Differential backups: what's changed since the last full backup, slower to make and faster to recover than incremental.
> * A good backup schedule combines full and incremental/differential backups at points in time, aiming at some objective.
-->

Let's start by defining backups.
A backup is a copy of your data from a specific point in time,
which is then securely stored and can be restored for usage.
The purpose of backups is to safeguard important data from loss, damage, encryption attacks, and other threats.

There are several modes of backups, each playing a role in a good strategy:

**Full backups**: This mode of backup creates a complete copy of all data.
It's the simplest type, being easy to keep and restore.
However, when there's a large amount of data, making a full backup takes the longest time,
and storing it takes just as much space.

**[Incremental backups](https://www.msp360.com/resources/blog/incremental-backup-guide/)**:
These backups save the data that has changed since the latest recent backup,
which itself can be full or incremental. 
That means that after a single full backup, there can be a series of incremental backups,
each saving the data that has changed in between.
Each incremental backup will take much less time to make and less space to store it.
However, when restoring data from a series of incremental backups, they all should be combined.
It takes extra time, thus making data recovery slower.

**[Differential backups](https://www.msp360.com/resources/blog/differential-backup-explained/)**:
This mode always copies all data that has changed since the latest full backup.
Such backups are slower to make than incremental ones, but instead, they provide better recovery time.

Now that we know the modes of backups and the relations between them, let's talk about the schedules.
A typical backup schedule combines full backups with incremental or differential to achieve a balance
between backup time, recovery time, and amount of storage required.
For example, a common approach is to do regular full backups — say, weekly or monthly,
and complement them with more frequent incremental or differential backups, done daily or several times per day.
We'll return to making the optimal schedule choice later in this article.

### Backup Retention Policy

<!--
> * The purpose of a retention policy is to ensure that backups are available when they're needed, thus giving reliability.
> * Retention policy dictates how long to keep backups, both full and incremental or differential.
> * There's an absolute minimum to store: the latest full backup plus what comes after it.
> * But it can provide insufficient reliability because data can get corrupted. Thus, it makes sense to store earlier backups.
> * "Naive" policy: store the latest N full backups.
> * FFI policy: short description and link.
> * GFS policy: short description and link.
-->

The purpose of a backup retention policy is to ensure that backups are available when they're needed,
providing reliability and security in data restoration.
A retention policy dictates how long the backups — both full and incremental or differential — are kept,
allowing service providers to make a balanced choice between reliability and costs.

**A minimal retention policy** should require storing the most recent full backup along with a series of
incremental backups or the latest differential backup.
This way, the data can be restored to the point of most recent backup.
However, this minimal approach may not provide sufficient reliability, because the most recent backups can be lost or damaged.
It's something that just happens sometimes, so we must account for this risk.
Therefore, it often makes sense to store earlier backups as well.
Let's have a look at the more advanced retention policies.

**The "straightforward" retention policy** should require storing the latest N full backups plus the incremental or 
differential backups made since the latest full backup.
Intermediate incremental or differential backups can be included as well.
Such a policy will provide much better reliability for restoring recent data versions, at the cost of using more storage space.

**[The Forever Forward Incremental (FFI) retention policy](https://help.mspbackups.com/backup/about/ffi/ffi)**
minimizes required backup storage by maintaining a single full backup,
followed by a series of incremental backups.
After reaching a certain number of incremental backups, earlier ones are merged into the full backup.
For example, we can have a full backup of the data we had a month ago, and then a series of daily incremental backups.
Once a week we would rebuild the full backup from the original data to make sure we have the right data.
Having all these backups, we can restore the data to the latest saved version, which will be no older than 24 hours.
But besides that, we can also restore the data to the state of any day in the recent month.
So, if we find out that our data was corrupted a few days ago, we can flexibly restore it to a point just before
the incident.
However, earlier times, such as a month or a year ago, are still inaccessible.

**[The Grandfather-Father-Son (GFS) retention policy](https://help.mspbackups.com/backup/about/gfs/gfs)**
organizes backups into short, medium, and long time cycles,
referred to as "son", "father", and "grandfather" backups, respectively.
For example, these can be daily, weekly, and monthly backups.
Daily backups are then kept for a week or two, weekly backups are kept for several weeks, and monthly backups — for several months.
With this approach, we always have redundant options for recovering a recent state of data,
but we're also able to recover the data from a few weeks or even months ago when we need this data.
If our data changes less frequently or we need to retain it for a longer time, we can use the weekly-monthly-yearly option.
Of course, using the GFS policy requires even more storage compared to simpler retention policies.
However, this storage is now used in a much more clever way, providing flexible options for data restoration.

Whatever retention policy you choose, you can balance it for reliability and recovery options,
considering the requirements for storage and resulting costs.
Next in this article, we'll see how to tailor a retention policy according to your business needs.

## How to Tailor Backup Schedule and Retention for Data Restoration Objectives

<!--
> * Data restoration is the ultimate goal of backing up data.
> * Restore Time Objective and Restore Point Objective.
> * Backup frequency improves RTO and RPO at the cost of storage and system performance.
> * Retention adds reliability, again at the cost of storage.
-->

We should start with one simple fact: customers never want to back up their data.
What they truly want is to be able to restore it when it's lost or damaged.
So, when we plan the backups and their retention, we should think of how we want to restore the data.

There are two key metrics in data restoration: Restore Time Objective (RTO) and Restore Point Objective (PRO).

**Restore Time Objective (RTO)** is the maximum allowable time to recover data and resume normal operations after an incident.
We know that restoring from a full backup is the fastest option while applying a long chain of incremental backups is the slowest.
So, the less RTO we need, the more frequently we have to make full backups.
**Restore Point Objective (PRO)** is the maximum allowable age of the data that the customer can lose in an incident.
Depending on the nature of the data, an acceptable RPO can be a week ago, or just some minutes ago.
It's usually more with documents and archives and much less with mission-critical databases.
A very recent RPO means that we need frequent backups, and some of these backups should likely be incremental or differential.

Increasing the frequency of backups can significantly improve both RTO and RPO when we need it.
However, this straightforward improvement comes at the cost of multiplied storage requirements.
If not managed properly, frequent full backups will bloat storage space and potentially lead to a situation when
there's no available storage left and no new backups can be made.
Here's why we need to use incremental or differential backups, along with an appropriate retention policy.

## How to Estimate Amount of Data Stored and Storage Cost

<!--
> * Now that we know the optimal schedule and retention policy, we can calculate the storage amount.
> * Think about the data modification rate, desired objectives, and reliability.
> * If you think the cost is too high, compare it with risks for restoring to an earlier point in time or not restoring at all.
> * If it seems cheap, consider retaining backups for a longer time or increasing frequency.
-->

It's time to draft the backup schedule and retention policy that will meet your or your customers' data restoration objectives.
You can do it in a few steps:

1.  Define the restoration objectives.
    Remember that they come from the nature of the data being safeguarded and the business requirements.
2.  Pick a backup schedule to meet the objectives.
    You can combine full backups with incremental or differential, depending on what your backup tools allow.
3.  Define an appropriate retention policy that will enable restoring the data in all required Restore Point Objectives.
    Note that according to the business requirements, these can be not just in the recent past, but even months or years ago.
    Retain a larger number of backups where you need more reliability and recovery options.
4.  Finally, calculate the amount of storage required for your selected backup schedule and retention.
    You can estimate by counting a full backup as the same storage that the original data requires,
    and a partial backup as a share, proportional to the rate of data modification.
    Say, if 1% of data is changed every day, a daily incremental backup will take about 1% of the size of a full backup.
    Knowing the estimated storage and the pricing of your storage provider, you can estimate the storage cost.

If the estimated storage cost seems too high, consider comparing it with the potential risks of not having a backup available,
or having it for an earlier point in time.
If you're not reaching the objectives with a given budget, it can mean two things: either the budget is insufficient,
or the objectives are overestimated.

On the other hand, if the storage cost seems affordable, consider retaining backups for a longer time or increasing
the frequency of backups.
Extending retention periods allows for more available recovery points.
This can be useful in cases where data corruption is discovered after several days or weeks, and already present in the most
recent backups.
Increasing the frequency improves recovery point objectives, allowing for restoration to a more recent point and fewer data loss.

Now that you have the schedule and retention policy, there are ways to improve them further:

*   Adjust the estimates for storage and restoration objectives based on real numbers.
    After you start backing up the data, you'll see the exact time it takes to make a backup and the storage that it takes.

*   Consider how the amount of data grows over time.
    As business grows, the amount of data grows with it.
    This growth will affect backup and restoration time, along with the required storage space.

## Example A: GFS policy for Mission-Critical Data

To illustrate the Grandfather-Father-Son policy, let's imagine an accounting database, which is critical for a company's business.
The current size of the database is around 100Gb, and we know that it gets around 100Mb of new data each week.
The database is updated many times a day, and in case of an incident, no more than a workday of data can be lost.
Daily transactions will be recovered manually.
We also know that our tools allow us to make incremental backups of the database.
Besides, law regulations require us to retain accounting information for at least three years.
What backup schedule and retention policy can we choose in this scenario?

Let's consider that we need both very recent and rather early points in time: from a day ago to three full years ago.
The first fact shows that we need to make backups at least daily.
The second fact dictates that we retain backups for more than three years.
All together, it brings us to the GFS retention policy.

Let's start with weekly full updates, made on Sundays.
They will require some time, but it won't affect performance, as it's done out of working time.
Then, we'll add differential backups done daily.
On Sundays, we'll make an incremental backup as well, so that we have two copies of data, in case one of them is damaged.
Once a month, we'll promote a weekly update to a monthly update, and once a year a monthly update will become a yearly update.

Next, we'll decide how much we want to retain.
We definitely need three yearly backups, as it's a requirement by law.
Then, we might keep a full year of monthly backups, because we know that financial data is very important during the year.
We'll retain three copies of weekly backups because the fourth copy becomes a monthly backup.
Let's also note that we remove older backups only after making new ones, so in fact we'll have up to 4 weekly backups at the same time.
And finally, we'll keep incremental backups for a whole month, because redundancy gives us extra reliability.
Let's count all of our backups now:

* Yearly (grandfather) backups: 3 * 100Gb
* Monthly (father) backups: 11 * 100Gb (not 12, because we've counted the last one as yearly)
* Weekly (son) backups: 4 * 100Gb
* Daily incremental backups, made on weekdays: 20 * 100Mb
* Total: 1820Gb.

After a year of working in the same way, the database will grow by another 5.4Gb, which brings us to 1925Gb total.
Now we can safely say that 2Tb is likely to suffice our needs for this year.

## Example B: FFI policy for Important Data

To illustrate the FFI policy, let's imagine a filesystem directory where the sales department keeps its work-in-progress contracts.
It's not large, amounting to about 10Gb, but a lot of new files are added and changed every day, amounting to around 100Mb of changes.
The size of the directory doesn't grow much, because completed contracts go to archives.
The sales department wants to be able to track the changes at least daily, as long as they were made no longer than eight weeks ago.
Oh, and files change even on weekends because, unlike accounting, salespeople seem to work all the time.

This looks very much like the forever forward incremental policy:
we care much about the recent changes, but after some time all we need is a full backup.
We also know that we'll remove the earlier full backup only after making a newer backup.

Let's count all of our backups:

* One full backup made exactly 56 days ago: 10Gb
* 55 daily incremental backups: 5.5Gb
* One full backup and one incremental backup made before removing the earlier ones: 10Gb + 100Mb.
* Total: 25.6Gb

Again, our estimate allows us to say that 30Gb is likely to suffice the storage needs.
But that amount of storage comes cheap, so maybe we can make salespeople happier by giving them
even better PTO and more reliability.
Say, we'll keep three Forever Forward monthly backups and all the incremental backups between them:

* Three full backups made exactly 28, 56, and 72 days ago: 30Gb
* 71 daily incremental backup: 7.1Gb
* One full backup and one incremental backup made before removing the earlier ones: 10Gb + 100Mb.
* Total: 47.2Gb

Now we keep almost twice the same data.
In exchange, we now have three very reliable restoration points in the form of full backups,
plus the ability to trace changes in the docs for 12 weeks. Neat!

## Conclusion

In conclusion, understanding the backup schedule and retention policy is crucial for data protection.
By carefully selecting the right backup types, schedule, and retention policy, you can ensure that your data is protected with enough reliability,
easily restored, and stored in a cost-efficient way.

This blog post has provided information about the various types of backups, ways to schedule them,
and the relationship between backup frequency and retention policies.
Whether you implement a simple straightforward retention policy,
the space-efficient Forever Forward Incremental approach,
or the most reliable and flexible Grandfather-Father-Son policy,
you will now be able to make the right choices and cost estimates.

<!--
## What's Next to Learn

> * Mastering backups doesn't stop on backup schedules and retention.
> * Plan your restoration strategy, considering your objectives. Consider different strategies for each type of data.
> * Check how your backups are restored. Do it regularly
> * Automate the backing up and restoration workflows.
-->

## Todo

- [ ] Links to other blog articles on related topics