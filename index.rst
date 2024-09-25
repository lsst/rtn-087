.. Review the README on instructions to contribute.
.. Static objects, such as figures, should be stored in the _static directory. Review the _static/README on instructions to contribute.
.. Do not remove the comments that describe each section. They are included to provide guidance to contributors.
.. Do not remove other content provided in the templates, such as a section. Instead, comment out the content and include comments to explain the situation. For example:
	- If a section within the template is not needed, comment out the section title and label reference. Do not delete the expected section title, reference or related comments provided from the template.
    - If a file cannot include a title (surrounded by ampersands (#)), comment out the title from the template and include a comment explaining why this is implemented (in addition to applying the ``title`` directive).

.. This is the label that can be used for cross referencing this file.
.. Recommended title label format is "Directory Name"-"Title Name" -- Spaces should be replaced by hyphens.
.. _Rubin-Observatory-CMMS-JIRA-Workflow-API:
.. Each section should include a label for cross referencing to a given area.
.. Recommended format for all labels is "Title Name"-"Section Name" -- Spaces should be replaced by hyphens.
.. To reference a label that isn't associated with an reST object such as a title or figure, you must include the link and explicit title using the syntax :ref:`link text <label-name>`.
.. A warning will alert you of identical labels during the linkcheck process.

.. See the `Documenteer documentation <https://documenteer.lsst.io/technotes/index.html>`_ for tips on how to write and configure your new technote.

##################################
CMMS-JIRA Workflow and API Details
##################################

.. abstract::

   Describes process diagram for maintenance activity planning and execution to develop tooling for Rubin Observatory operations.


.. _CMMS-JIRA-Workflow-API-Introduction:

Introduction
============

.. This section should provide a brief, top-level description of the page.

.. Important::

    This document is currently under development.
    It should not be used nor consulted for a place of information at this time.

The purpose of this document is to capture details related to the function of the workflow in OpenMAINT and the API between OpenMAINT and Jira.

.. note::
   Final workflow diagram will be uploaded as a PDF to docushare, and a link needs to be included in this document for access.
   We can make this a "document" just to allow an easy place to reference, or release as an RTD if we want more stringent version control.

The version of this document corresponds to Version 1 of the workflow diagram.
The screenshots within this docushare are for reference.

.. See (link) for the workflow.

.. note::
   This document primarily describes functions within OpenMAINT and how it interacts with Jira.
   We will generate a separate document with a quick reference guide for users, which will cover how personnel interact with the systems to execute preventive maintenance activities.

.. _CMMS-JIRA-Workflow-API-Actions:

Actions taken by OpenMAINT in JIRA
==================================

*This interaction is one-way only.
OpenMAINT feeds info to JIRA, no info flows back.*

|

.. figure:: /_static/open-jira-ticket.png
    :name: open-jira-ticket

Initial opening of the JIRA ticket.
This occurs one month before the schedule maintenance time in OpenMAINT to allow for resource scheduling and activity coordination in Jira.
The initial info transferred to the JIRA ticket should include:

* Assignee (this is the group lead for the assigned group, or the default manager (Eduardo) if that person doesn’t exist in JIRA).
* Add Manager (Eduardo) as a watcher
* Priority (still need to match these with JIRA values?).
* Target execution date of the maintenance used as the Due Date in Jira.
* Link to OpenMAINT maintenance activity.
* Suggested text for body of the ticket:
  This is a maintenance activity from OpenMAINT.
  The group lead should confirm their group will be doing this work, then assign an appropriate technician/point-of-contact in JIRA, and get the activity in the work schedule.
  The technician/point-of-contact will START PROGRESS on the JIRA ticket, and then move to OpenMAINT to perform the work.
  Within OpenMAINT, the technician/point-of-contact will execute the maintenance activity, follow activity instructions provided, record progress, and close the maintenance activity.
  They do not need to return to JIRA, all updates will be automatic.
  See (link to document or confluence page) for reminders on how to execute the workflow in OpenMAINT.

|

.. figure:: /_static/CMMS-cancel-and-comment-Jira.png
    :name: CMMS-cancel-and-comment-Jira

If a maintenance activity is rejected and closed by the Group Leader, the Jira ticket is cancelled. 
OpenMAINT will also add a comment that says “This maintenance activity has been rejected by the Group Leader and will be skipped.”

|

.. figure:: /_static/CMMS-reassigns-jira-ticket.png
    :name: CMMS-reassigns-jira-ticket

Re-assigning the JIRA ticket if the Team is changed in OpenMAINT (ticket is assigned to the group lead or the default manager).
The Team should only be changeable by the Group Leader.

|

.. figure:: /_static/CMMS-changes-Jira-status-review.png
    :name: CMMS-changes-Jira-status-review

When the technician has finished updating the preventative maintenance activity and sends it for review, OpenMAINT will transition the Jira ticket from “IN PROGRESS” to “IN REVIEW”.
It will also add the group leader as the reviewer.
It will also make a comment saying,
“The maintenance activity has been completed.
The outcome was [Outcome].
See OpenMAINT for additional details.”

.. note::
   Rubin Team: We need to look at whatever other fields are required in JIRA to transition maintenance tickets in Jira from In Progress to In Review.
   Then make sure those details are filled by OpenMAINT so that the transition doesn’t fail.

|

.. figure:: /_static/CMMS-changes-Jira-status-progress.png
    :name: CMMS-changes-Jira-status-progress

If the Group Leader changes the OpenMAINT ticket back to in progress (i.e., takes it out of review and sends it back to the technician for additional work), OpenMAINT will transition the Jira ticket from “IN REVIEW” to “IN PROGRESS”.

|

.. figure:: /_static/CMMS-changes-Jira-status-closed.png
    :name: CMMS-changes-Jira-status-closed

If the Group Leader closes the OpenMAINT ticket, OpenMAINT will automatically change the status of the Jira ticket to “CLOSED”.
It will also add a comment depending on the final status of the maintenance activity:

* If the OUtcome is “Maintenance Successful”, the comment will say “This maintenance activity has been closed.
  All tasks were completed successfully. See OpenMAINT for additional details.”
* If the Outcome is “Maintenance Not Completed”, the comment will say “This maintenance activity has been closed. 
  There were problems, and all tasks were NOT completed successfully. See OpenMAINT for additional details.”
* If the Outcome is “Maintenance Not Required”, the comment will say “This maintenance activity has been closed. 
  It was determined this maintenance is not required. See OpenMAINT for additional details.”

|

.. figure:: /_static/skipped-comment.png
    :name: skipped-comment

If the Group Leader decides to skip the next scheduled maintenance activity, the corresponding Jira ticket should be canceled, with a comment added saying “This scheduled maintenance activity has been skipped.”

|

.. figure:: /_static/update-due-dates-in-JIRA.png
    :name: update-due-dates-in-JIRA

If the schedule of a maintenance activity is updated in OpenMAINT, the due date of the corresponding Jira ticket will be updated to match.
The comment added to the Jira ticket will depend on what changes were made to the schedule:

* If the cadence was maintained, a comment should be added that says “The due date was changed from [old due date] to [new due date].”
* If the maintenance activity schedule was paused, a  comment should be added that says “This activity has been paused until [restart date]. 
  It has been paused for this reason: [insert reason provided by Group Leader within OpenMAINT]”

|

.. figure:: /_static/CMMS-posts-comment-in-JIRA.png
    :name: CMMS-posts-comment-in-JIRA

OpenMAINT will add comments to the Jira ticket throughout the workflow, when certain actions are taken within OpenMaint.
In addition to the comments already mentioned that go along with specific actions taken by OpenMAINT, these include:

* If the assignee has been changed in OpenMAINT, make a comment saying “The OpenMAINT assignee has been changed from [old assignee] to [new assignee].”
* When the technician executes the maintenance activity, make a comment saying “The preventative maintenance activity has been executed.”
* If the preventative maintenance activity is suspended, make a comment saying “The preventative maintenance activity has been paused.”

|

.. _CMMS-JIRA-Workflow-API-Features:

Features within OpenMAINT
=========================

.. figure:: /_static/execute.png
    :name: execute

In the “Acceptance” stage of OpenMAINT, the technician ONLY has the option to “Execute” the preventative maintenance activity. 
The Group Leader is the only one with the power to reject and close. 
If the technician is busy or thinks they’re not the right person for the job, they work with the Group Leader to reschedule and/or choose a new assignee in Jira.

|

.. figure:: /_static/CMMS-ticket-review.png
    :name: CMMS-ticket-review

The technician doesn’t have the option to conclude the activity, instead they have the option to Send for Review. 
When the technician sends the maintenance activity for review, they should be required to enter the outcome, and the completion date of the work.
It should be clear that this is the date that physical work was completed, so they don’t update it if they have to go back and add paperwork.
The technician has 3 options when selecting the Outcome: Maintenance Successful, Maintenance Not Completed, and Maintenance Not Required.

|

.. figure:: /_static/CMMS-ticket-review-for-closure.png
    :name: CMMS-ticket-review-for-closure

After the OpenMAINT maintenance activity ticket has been sent for review, only the Group Leader should have edit access.

|

.. figure:: /_static/Group-Leader-approval-choice.png
    :name: Group-Leader-approval-choice

(Note that the wording in this screenshot needs to be updated to better reflect the language that's in OpenMAINT) 
After reviewing the completed maintenance activity, the Group Leader has the action options to Conclude Activity or Send for Rework. 
Send for Rework opens up edit access to the Technician again. 
Ideally the original Outcome and completion date will be preserved, and can be updated if necessary when the Technician sends for review again.


|

.. figure:: /_static/CMMS-popup-window.png
    :name: CMMS-popup-window

When the Group Leader closes the OpenMAINT ticket, a pop-up window should ask them how they want to adjust the schedule for the next maintenance activity.
The pop-up should include the date of the next scheduled maintenance, and the typical maintenance period of this activity.
They should be allowed to choose one of the following options:

* **Maintain Date** maintains the current schedule

  * No due dates are adjusted with this option.
  * If the normal cadence is maintenance once a month and the next scheduled activity is 2 weeks after maintenance was last completed, the due date will still be in 2 weeks.

* **Maintain Cadence** maintains the activity frequency and adjust the schedule

  * Due dates for all future maintenance activities on the schedule are updated to maintain the normal cadence of the maintenance activity.
  * If the normal cadence is once a month, the next maintenance activity will be rescheduled to be due 1 month after the last maintenance activity was completed.

* **Skip Next** cancels the next maintenance activity and maintains the rest of the schedule

  * (NOTE: This will override any schedule changes in Jira)
  * The next maintenance activity is skipped, and the schedule for the remaining maintenance activities stays the same.

* **Pause** is selected if this activity won't be done for a while. This option reschedules the next maintenance activity based on the selected date.

  * The Group Leader will be prompted to select or enter a date when the maintenance activity will resume.
  * The Group Leader will be required to write a comment saying why the maintenance activity is being paused.

For Reference: User Intractions
===============================
*While the information in this section does not directly impact the API or functionality within OpenMAINT, we feel it is helpful to provide some context with how we intend users to interact with these program features.*

|

.. figure:: /_static/tech-tasks.png
    :name: tech-tasks

The technician will perform the maintenance activity and update the OpenMAINT ticket regardless of how the maintenance activity goes. 
This includes whether everything went perfectly, something broke, the maintenance wasn’t required, etc. 
The intention is to use the maintenance activity to record what happened, so the Group Leader can review everything in one place and decide what to do next. 
We need to make sure options for entering data, comments, and outcomes are flexible enough to handle many different scenarios.

|

.. figure:: /_static/Group-Leader-approval-tasks.png
    :name: Group-Leader-approval-tasks

The Group Leader’s role in reviewing and closing out completed maintenance activities is very important. When reviewing the ticket, they must:

* Review the Outcome, make sure they agree with it
* Make sure any necessary attachments are included
* Review any notes from during the activity

  * Did something go wrong? Did something break? Do we need to generate a FRACAS ticket and/or corrective action?
  * Were redlines made to the procedure? Do we need to make an action item to finalize those updates?

* Is the maintenance activity completely filled out, is something missing? Was something not done correctly? Does this need to be sent back to the technician for additional work?

When closing the maintenance activity once everything looks good, the Group Leader must then make decisions about scheduling the next maintenance activity.

