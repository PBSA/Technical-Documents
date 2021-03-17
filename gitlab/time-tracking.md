---
description: >-
  How to use GitLab quick actions for estimates, time spent and weights on
  issues.
---

# Time Tracking

[https://docs.gitlab.com/ee/user/project/quick\_actions.html](https://docs.gitlab.com/ee/user/project/quick_actions.html)

Quick actions are text-based shortcuts for common actions that are usually done by selecting buttons or dropdowns in the GitLab user interface. You can enter these commands in the descriptions or comments of issues, epics, merge requests, and commits.

Be sure to enter each quick action on a separate line to allow GitLab to properly detect and execute the commands.

## Selecting an Issue

1. Navigate to the project of choice in GitLab.
2. Click on the `issues` menu option

![](../.gitbook/assets/image%20%2866%29.png)

3. Select the issue of choice.

## Time Tracking

Lead developers who will assign issues should make sure to provide a time estimate and weight to them. They should also ensure these issues are attached to the appropriate milestones and epics. 

The purpose of this is to collect metrics and be able to provide a better timeline for project delivery.

### Estimates

To estimate an issue in GitLab, quick actions can be utilized. Inside the comment section of an issue type the following command: `/estimate <1w 3d 2h 14m>`. 

![Providing an 8 hour estimate to an issue](../.gitbook/assets/image%20%2868%29.png)

Once an estimation is provided, the issue will update to reflect the submitted comment.

![Issue updated based on time estimate](../.gitbook/assets/image%20%2864%29.png)

### Spent Time

To track the time spent on an issue in GitLab, quick actions can be utilized. Inside the comment section of an issue type the following command: `/spend <time(1h30m|-1h30m)> <date(YYYY-MM-DD)>` to add or subtract spent time.

![Providing a 2 hour time spend](../.gitbook/assets/image%20%2871%29.png)

Once the spent time is provided, the issue will update to reflect the submitted comment.

![Issue updated based on time spent](../.gitbook/assets/image%20%2862%29.png)

### Weights

When you have a lot of issues, it can be hard to get an overview. By adding a weight to each issue, you can get a better idea of how much time, value or complexity a given issue has or costs.

Weight points can be associated for each estimated hour. So a task with an estimate of 8 hours and be associated 8 points to the weight. 

To add weight to an issue in GitLab, quick actions can be utilized. Inside the comment section of an issue type the following command:  `/weight 0, 1, 2...`.

![Providing 8 points to weight](../.gitbook/assets/image%20%2860%29.png)

Once the weight is provided, the issue will update to reflect the submitted comment.

![](../.gitbook/assets/image%20%2863%29.png)

### Milestones

Utilizing time tracking on issues and attaching them to milestones also updates the time tracking of that milestone for all issues attached. 

![Milestone with attached issues that have time tracking](../.gitbook/assets/image%20%2870%29.png)



