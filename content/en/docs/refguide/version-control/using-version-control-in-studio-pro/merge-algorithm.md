---
title: "Combining Changes and Conflict Resolution"
linktitle: "Combining Changes and Conflict Resolution"
url: /refguide/merge-algorithm/
category: "Version Control"
weight: 10
description: "Describes combining changes with conflict resolution flow."
tags: ["rebase", "mine", "theirs", "merge", "conflict"]
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
aliases:
    - /refguide/new-merge-algorithm
---

## 1 Introduction

While working on your changes you may find that your local copy of the app model doesn't have all the changes that other team members have [committed](/refguide/commit-dialog/) to the server (the [Mendix Team Server](/developerportal/general/team-server/), or an [on-premises server](/refguide/on-premises-git/)).
In Git terminology this is called 'being behind'.

When this happens, Mendix Studio Pro offers two ways to combine your changes with changes from the server: [Rebase](#rebase) and [Merge commit](#merge). 

Both options support the following features when it comes to [resolving the conflicts](#resolve):

* **Fine-grained conflict resolution** – When there are conflicting changes in a document, you do not have to choose between whole documents, resolving a conflict using your change or using their change. Instead, you can resolve conflicts at the level of individual elements, such as widgets, entities, attributes, or microflow actions. All non-conflicting changes from both sides are accepted automatically.
* **No conflicts on changes to lists of widgets** – When two developers make changes to widgets in the same document there is no conflict, the changes are combined. However, if the changes are made too close to the same place in the document, a **list order conflict** is reported that reminds the developer who is merging the changes to decide on the final order of the widgets in the list.
* Can be aborted at any time. Studio Pro will continue from your latest local commit.

The differences between the two approaches are as follows:

* Rebase (*default*): 
    * Treats changes from the server as leading, by first retrieving the server state and then reapplying your work to it. 
    * Results in a simple commit history.
    * Resolves conflicts when reapplying your local commits. If you have 3 local commits, this could trigger conflict resolution 3 times.
* Merge commit: 
    * Treats local and remote changes equally, and combines them in a separate 'merge commit'.
    * Results in a more complicated commit history with extra merge commits.
    * Resolves conflicts once, regardless of the number of local and remote commits being merged.

{{% alert color="info" %}}
In general, Mendix recommends using the Rebase strategy when combining changes, especially when actively collaborating on the same branch.

In exceptional cases, for example when you have a lot of local commits where you expect conflicts, a merge commit might be the better choice. 
{{% /alert %}}

Both processes are guided by [notification controls](#notifications) showing actual state and possible next steps.

## 2 Scenario {#scenario}

The clearest way to explain and illustrate the differences between Rebase and Merge commit when combining changes, is to examine an example scenario. The sections [Rebase](#rebase) and [Merge commit](#merge) show how the two approaches work.

### 2.1 Starting point 

There are two entities `User` and `Game` which you have added to the domain model of your project.

The User entity includes string attributes `E_mail` and `Second_E_mail`.

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/DomainModel/Starting_State.png" >}} 

### 2.2 Local changes, your work 

During your work you make two changes, each one in separate commit. 

In the first commit you rename `E_mail` to `Email` 

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/DomainModel/First_Local_Commit.png" >}} 

In the next commit you rename `Second_E_mail` to `Second_Email` to be consistent with previous change. 

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/DomainModel/Second_Local_Commit.png" >}}   

### 2.3 Server Changes

In the meantime, your colleague also decided to make some changes to both email fields. They have renamed `E_mail` to `EmailAddress` and removed `Second_E_mail` entirely. These changes have been pushed to the server.

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/DomainModel/Remote_State.png" >}} 

### 2.4 Summary 

The current situation could be represent as shown below. 

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/Steps/Rebase_Starting_state.png" alt="Team Server with three commits (1, 2, and 4), while in Studio Pro there are also three commits (1, 3, and 5)" width="525" >}} 

## 3 Combining Changes

This section goes through the [example scenario](#scenario) with the two different approaches: [Rebase](#rebase) and [Merge commit](#merge). 

Every time changes can be combined, for example when pulling changes from the server, you can choose the approach. The default can be changed by adjusting the [user preference](/refguide/preferences-dialog/#git).

### 3.1 Rebase {#rebase}

Rebasing is the default way to integrate your work with the server changes. It moves your changes to the tip of the changes pulled from the server.

{{% alert color="info" %}}
During the rebase process, there is a slight terminology change in Studio Pro compared to merge.
{{% /alert %}}

The rebase process is described below, starting with the state described in the [example scenario](#scenario), above.

#### 3.1.1 Rebase Started 

After starting the rebase, your two commits (#3 and #5) are temporarily put aside, and Studio Pro shows the latest changes which were pulled from the server (which includes commits #2 and #4).

{{% alert color="warning" %}}
Your work will be now referenced as 'Theirs', while the server changes will be 'Mine'. This is opposite to the way that the work is labelled for a merge commit.
{{% /alert %}}

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/Steps/Rebase_Step_1.png" width="525" >}} 

#### 3.1.2 Resolving the First Conflict  

Git tries to apply your first commit (`#3`) to the tip of the rebasing branch ('Mine'). The commit will come after commit `#4`. 

If there were no conflicts when comparing your commit (`#3`) with the latest state from the server (`#4`), Studio Pro automatically continues. A new commit would be created from your commit, shown as commit `#3'` in the image below. The process would then continue with the next commit (`#5`).

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/Steps/Rebase_Step_2.png" width="525" >}} 

In our example, however, there is a conflict as the `E_mail` attribute was renamed both on the server, and in your local work.

In the **Changes** pane you can see your change in the 'Theirs' column while your colleague's work is shown in the 'Mine' column. 

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/Conflicts/Rebase_First.png" >}} 

You need to [resolve the conflict](#resolve) to proceed with the rebasing process. After resolving the conflict you can amend the current commit message and commit `#3'` is created. 

Now Studio Pro will continue with rebasing the next local commit (`#5`).

#### 3.1.3 Resolving the Second Conflict 

While Rebasing the next commit (`#5`) another conflict is detected. You can choose to resolve this conflict using either 'Theirs' or 'Mine' or reverting to the original.

You can also make additional changes which are added to the same commit. For example you can add another attribute `Login`, to the `User` entity. These changes will be represented as 'Mine', together with changes that were taken from the server. 

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/Conflicts/Rebase_Mine_Change.png" >}} 

Once the conflict is resolved and you continue the rebase, a new commit (`#5'`) is created from your commit (`#5`), and you can optionally amend the commit message. 

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/Steps/Rebase_Step_3.png" width="525" >}} 

As this was the last local commit to reapply, the rebasing can now be completed.

#### 3.1.4 Test Changes 

Once the rebase process is completed, the original commits (`#3` and `#5`) that were put aside are now removed. The final state of the branch has the commits `#1`, `#2`, `#4`, `#3'`, and `#5'`, while the server still only has commits `#1`, `#2`, and `#4`.
 
{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/Steps/Rebase_Step_4.png" width="525" >}}

Your work is still on your local machine and you should test whether the combined state works as expected.

#### 3.1.5 Push Changes 

After testing the merged changes, push your work to the server to set the server state to the same as your local state.

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/Steps/Rebase_Step_5.png" width="525" >}}

{{% alert color="info" %}}
If a colleague has pushed other changes to the server while you were working on the merge, Studio Pro will again ask how to combine your current work (including the new `#3'` and `#5'` commits) with the latest changes on the server.
{{% /alert %}}

### 3.2 Merge commit {#merge}

Merge commit is an alternative way to integrate your work with remote changes. The combined state is committed using a separate special merge commit.

The merge commit process is described below, starting with the state described in the [example scenario](#scenario), above.

#### 3.2.1 Merge Started 

After starting the merge process Studio Pro will combine your local work (`#3` and `#5`) with the state of the server (`#2` and `#4`). 

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/Steps/Merge_Step_1.png" width="525" >}} 

{{% alert color="warning" %}}
Your work shall be now referenced as 'Mine', while server changes shall be 'Theirs'. This is opposite to the way that the work is labelled for a rebase.
{{% /alert %}}

In the end you'll be creating a merge commit that merges commits `#2` and `#4` into your work. The changes already in your local work (`#3` and `#5`) are kept.

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/Steps/Merge_Step_2.png" width="525" >}} 

#### 3.2.2 Conflict resolution: resolving the two conflicts

If conflicts arise between any local and remote commits, you must resolve them before creating the merge commit for the combined state. Unlike rebasing, which requires conflict resolution for each local commit with conflicts, here you have only one round of conflict resolution.

**Renaming of `E_mail` attribute**

As the `E_mail` attribute was renamed on both the server and in your local work, you need to decide which changes to retain, or make yet another version. In the **Changes** pane you can see your change in the 'Mine' column while your colleagues' work is shown in the 'Theirs' column. 

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/Conflicts/Merge_First.png" >}} 

**Removal of `Second_E_mail` attribute on server**

Because the `Second_E_mail` attribute was removed on the server, while renaming it in your local work, there is another conflict which needs to be resolved.

You can also make additional changes which are added to the same commit. For example you can add another attribute `Login`, to the `User` entity.

After resolving all conflicts you can proceed with testing the app.

#### 3.2.3 Test and Push Changes 

When the combined state is tested, you can commit the current state of the app. This is a new commit (`#6`) which will always show that it merged commits `#3` and `#5`.

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/Steps/Merge_Step_3.png" width="525" >}} 

By default Studio Pro will also push your work to the server when making a commit.

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/Steps/Merge_Step_4.png" width="525" >}} 

{{% alert color="info" %}}
If a colleague has pushed other changes to the server while you were working on the merge, Studio Pro will again ask how to combine your current work (including `#6`) with the latest changes on the server.
{{% /alert %}}

### 3.3 Summary

You have merged your local work with the latest state from the server and resolved all conflicts. 
For rebasing this meant two rounds of conflict resolution, while for a merge commit you had a single round with all of the conflicts being resolved at the same time.

In the end the history on the server will look like this:

* After a Rebase:
    {{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/Steps/Rebase_End_state.png" alt="All commits in the order #1, #2, #4, #3', and #5'" width="525" >}}
* After a Merge commit:
    {{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/Steps/Merge_End_state.png" alt="Commits #1, #2, #4, and #6, with commit #6 including commits #3 and #5" width="525" >}}

So rebasing results in a simpler commit history, while a merge commit results in an additional commit that will always show as containing another commit or set of commits.

## 4 Resolving conflicts {#resolve}

When you need to resolve conflicts, Studio Pro enables this in two ways: 

* Interactive merge
* Using whole documents

While explaining resolving of conflicts we will be using [merge commit](#merge) flow. You can resolve conflicts during a rebase in the same way except that 'Mine' and 'Theirs' are reversed.

### 4.1 Using Interactive Merge

For the conflict, you can inspect the changes and decide which version to apply. Select the line that represents the conflict and choose **Resolve using Mine** or **Resolve using Theirs**.

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/Conflicts/Interactive_Merge.png" >}}

You will see the document update immediately after you click the button. If you are not satisfied with your choice, you can use undo to go back and try another option. 

{{% alert color="info" %}}
You must click the document to bring it into focus before you can use the keyboard shortcuts <kbd>Ctrl</kbd>+<kbd>Z</kbd> and <kbd>Ctrl</kbd>+<kbd>Y</kbd> to undo or redo your choice.
{{% /alert %}}

There is a third option to deal with a conflict: **Mark as Resolved**. This means that you do not choose either side to resolve the conflict and keep things the way they were in the original. Neither of the new text changes will be applied.

Once you have chosen one of the three options to resolve the conflict, green check marks will appear to indicate that this conflict has been dealt with.

Once all conflicts have been resolved, click the **Accept and Exit** button to finalize the results. The document will be saved and the conflict for that document will be gone. The result is the document that contains changes from both sides and possibly some manual edits.

At any time, you can also choose to abort conflict resolution by clicking the **Cancel** button. The conflict will remain, and you can resolve it later.

### 4.2 Using Whole Documents 

You can resolve conflicted documents too. There are two cause for document conflicts:

1. One person deletes a document and the other makes a change inside that document.
2. Both people move a document but to different places in the app tree.

The involved document is marked as conflicted and you can see the reason in the details column of the **Changes** pane. You can resolve the conflict by one of the following:

* using my whole document - while merging it will resolve all conflicts in this document using your work
* using theirs whole document - while merging it will resolve all conflicts in this document using server changes

{{% alert color="warning" %}}
Remember that Mine and Theirs are different, depending on whether you are using rebase or merge commit.
{{% /alert %}}

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/Conflicts/Interactive_Merge.png" >}}

{{% alert color="info" %}}
If a whole folder (or module) was *deleted* and another person changes a document inside that folder, the folder/module is restored and also marked as conflicted. This way you know that the intention was to delete that folder but it has been restored to show you the context of the changed document.
{{% /alert %}}

## 5 Notification Controls {#notifications}

While combining changes either by rebase or merge you will be shown a notification bar which will inform about current status of the process and next available steps.

Some of the controls are shared between both processes

### 5.1 Shared

#### 5.1.1 'Abort merge'

This button shall always be visible on almost all steps.

When clicked and confirmed combining process shall be stopped and all of the changes done prior to entering it shall be removed.
In other words you shall be resetted to the point before starting this process.

In has no effect on the server, only on your local work.

#### 5.1.2 'Show conflicts'

This button is only visible when there are conflicts related to your application, i.e. domain model, microflows.

When clicked it will bring 'Changes Pane' into the view, as this is a place in which you should resolve this type of conflicts.

#### 5.1.3 'Show file conflicts'

Compare to above button, this one shows up when there are conflicts in file not directly linked to your application.

When clicked it will open up a pop up window with list of all the files that are affected by merge, with conflicted ones at the top of the list.

You can also use ['Show Changes on Disk'](/refguide/version-control-menu/#4-show-changes-on-disk) menu item.

### 5.2 Rebase Specific Controls

Rebase notification bar while in conflicts phase.

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/NotificationBars/Rebase_Conflicts.png" alt="Rebase notification bar while in conflicts" width="525" >}}

Rebase notification bar when conflicts for current step resolved.
{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/NotificationBars/Rebase_Continue.png" alt="Rebase notification bar when ready for next step" width="525" >}}

Rebase notification bar when whole rebase concluded.
{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/NotificationBars/Rebase_Conclude.png" alt="Rebase notification bar while complete" width="525" >}}

#### 5.2.1 'Continue'

This button is visible when there are no more conflicts to resolve but there are still other changes to rebase.
When click it resumes rebase process.

#### 5.2.2 'Push'

This button is visible after rebase has finished successfully and your changes are ready to be pushed to the server.
When clicked it triggers a ['push'](/refguide/version-control-menu/#4-push) operation.

### 5.3 Merge Specific Controls

Merge notification bar while in conflicts phase.

{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/NotificationBars/Merge_Conflicts.png" alt="Merge notification bar while in conflicts" width="525" >}}

Merge notification bar when merge concluded.
{{< figure src="/attachments/refguide/version-control/using-version-control-in-studio-pro/combining_changes/NotificationBars/Merge_Conclude.png" alt="Merge notification bar while complete" width="525" >}}

#### 5.3.1 'Commit'

This button is visible when there are no more conflicts to resolve, and merge process is finished.

When clicked it opens up [commit dialog](/refguide/commit-dialog/) with predefined message indicating that this is a merge commit.