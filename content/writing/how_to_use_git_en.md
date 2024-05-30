---
title:  "How to use Git"
date:   2022-05-12 12:00:00
categories: new
tags: [Git, F***, Coding, type/write]
---

How to use Git
================

Programmers use Git every day, but most people are still using basic commands like commit, push, pull; and when needing to be a participant or starting to manage the repository and Version Releases, it starts to get awkward.

Here I would like to share some stories in the company that I have encountered.

Case 1: Choose feature to release.
----------------

A project that combines both mobile and backend, a customer can request a lot of features during a Sprint or a phase of the project. However, from the demand from the marketing department, many features may not be selected for golive, but the Test team still needs a version with full features to complete the task.

From the beginning of the project, for quite a long time, the project used the release branch workflow, of course due to the initial situation of the project at that time, just moved all the features from the ***develop*** environment. to ***staging*** then to ***production***.

When the client arose the above need, the mistake was made, the developer tried to use cherry-pick to select the commits from the staging branch to production, it was obvious that there was a very high chance that the commit would be lacking.

![alt text](../assets/img/Git-Page-1.png)

### So what did the team do?

It is easy to see the need here to maintain the source code between independent features/tasks/fixbugs. Besides, these branches still merge into the develop branch for verifying features and maintaining the latest live version.

![alt text](../assets/img/Git-Page-2.png)

To make it easy to understand, we still code on the feature branch as usual and merge but the source code in the feature is then more complete (after the tester has tested it) and merge when that feature or bug (this bug is a detected bug). currently in production) is decided to release, let's call this branch ***staging***
Previously the version that tester received to verify feature or bug would be merged more often like develop branch like in old git flow. (temporarily called ***develop***)

![alt text](../assets/img/Git-Page-3.png)

We deal with follow-up questions related to daily team activities.

#### New features

There is no different with the old way.

#### Fix bug
So how are bugs handled? The principle is also very simple, but the implementation is a bit more complicated.
* Bugs where will be fixed there. That is, when you find a bug somewhere, it will be fixed there first: for example, the above golived version or develop or staging
* Identify the source of the bug
  * It's best to identify which commit the bug was created from or when the team was working on something.
  * If it can't be determined (possibly due to many reasons leading to the bug), it is possible to identify the feature or fix the bug that leads to the degradation.
* After identifying the above 2 points and patching the bug in the branch we discovered (split branch to fix bug and merge)
  * Cherry-pick merge commits on related branches:
    * For example, you find a bug in version 2.0.1, but 2.0 and 1.0 are 2 versions running in parallel (1.0 is still LTS) but you identify a bug that appears from 1.0, you still have to patch it for 1.0, however in this case sometimes we often consider it as 2 bugs for ease of handling
    * More common case is bug in staging (tester branch), since ***staging*** is not merged with ***develop*** we need to cherry-pick on ***develop*** and the branch that implements that feature.
    * If the feature has been released in the golived version, you only need to care about ***develop***, ***staging***, but actually before that, the bug fix branch was also implemented as shown in the 3rd picture. .


![alt text](../assets/img/Git-Page-4.png)

So it can simply be summed up as follows: when a bug is discovered, fix it right there and cherry-pick related branches that have not been released: ***staging***, ***develop*** is require.

#### Release

Quite simply, when the source code has been tested in ***staging*** and cherry-picks the patches on the ***feature*** branch and fixbug, it means the source code in those branches is complete. and ready to release, when needed, any functionality can be completely merged into the ***production*** branch or more carefully, you can add the ***pre-production*** branch to use so that users can check again one last time.
