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

*This interaction is largely one-way. 
OpenMAINT feeds info to Jira, and Jira is not set up to feed information back. 
Any information transfer from Jira back to OpenMAINT will be pulled by OpenMAINT.*

|

.. figure:: /_static/open-jira-ticket.png
    :name: open-jira-ticket
    :width: 380 px

Initial opening of the Jira ticket.
This occurs one month before the schedule maintenance time in OpenMAINT to allow for resource scheduling and activity coordination in Jira.
The initial info transferred to the Jira ticket should include:

* Assignee (this is the group lead for the assigned group, or the default manager (Eduardo) if that person doesn’t exist in JIRA).
* Add Manager (Eduardo) as a watcher
* Components: Planned Maintenance
* Target execution date of the maintenance used as the Due Date in Jira.
* Link to OpenMAINT maintenance activity.
* Suggested text for description of the ticket:
  This is a maintenance activity from OpenMAINT: (name of maintenance activity)
  The group lead should confirm their group will be doing this work, then assign an appropriate technician/point-of-contact in Jira, and get the activity in the work schedule.
  The technician/point-of-contact will START PROGRESS on the Jira ticket, and then move to OpenMAINT to perform the work.
  Within OpenMAINT, the technician/point-of-contact will execute the maintenance activity, follow activity instructions provided, record progress, and send the maintenance activity for review.
  They do not need to return to Jira, all updates will be automatic.
  See (link to document or confluence page, TBD) for reminders on how to execute the workflow in OpenMAINT.

.. note::
   In the future, we want to consider including a Priority on maintenance activities.
   This is already a feature within Jira.
   To implement this, we would need to determine how a priority label could be integrated into OpenMAINT maintenance activities.

|

.. figure:: /_static/CMMS-cancel-and-comment-Jira.png
    :name: CMMS-cancel-and-comment-Jira
    :width: 550 px

If a maintenance activity is rejected and closed by the Group Leader, the Jira ticket is cancelled. 
OpenMAINT will also add a comment that says “This maintenance activity has been rejected by the Group Leader and will be skipped.”

|

.. figure:: /_static/CMMS-reassigns-jira-ticket.png
    :name: CMMS-reassigns-jira-ticket
    :width: 550 px

Re-assigning the JIRA ticket if the Team is changed in OpenMAINT (ticket is assigned to the group lead or the default manager).
The Team should only be changeable by the Group Leader.

|

.. figure:: /_static/CMMS-changes-Jira-status-review.png
    :name: CMMS-changes-Jira-status-review
    :width: 530 px

When the technician has finished updating the preventative maintenance activity and sends it for review, OpenMAINT will transition the Jira ticket from “IN PROGRESS” to “UNDER REVIEW”.
It will also add the group leader as the reviewer.
It will also make a comment saying,
“The maintenance activity has been completed.
The outcome was [Outcome].
See OpenMAINT for additional details.”

.. note::
   There is currently no reviewer field on MAINT Jira tickets.
   We will work on adding this, so it should be planned for in the API even if it can't be implemented immediately.

|

.. figure:: /_static/CMMS-changes-Jira-status-progress.png
    :name: CMMS-changes-Jira-status-progress
    :width: 530 px

If the Group Leader sends the OpenMAINT ticket back (i.e., takes it out of review and sends it back to the technician for additional work), 
OpenMAINT will transition the Jira ticket from “UNDER REVIEW” to "REJECTED", and then to “IN PROGRESS”. 
It will also leave a comment based on the action the Group Leader selected:

* If the Group Leader selected Send Back (Add report), the comment will say "This maintenance activity has been sent back. 
  Additional paperwork is required. See OpenMAINT for details."
* If the Group Leader selected Send Back (Rework), the comment will say "This maintenance activity has been sent back. 
  Rework is required. See OpenMAINT for details."

|

.. figure:: /_static/CMMS-changes-Jira-status-closed.png
    :name: CMMS-changes-Jira-status-closed
    :width: 550 px

If the Group Leader closes the OpenMAINT ticket, OpenMAINT will automatically change the status of the Jira ticket to “CLOSED”.
It will also add a comment depending on the final status of the maintenance activity:

* If the Outcome is “Maintenance Successful”, the comment will say “This maintenance activity has been closed.
  All tasks were completed successfully. See OpenMAINT for additional details.”
* If the Outcome is “Maintenance Not Completed”, the comment will say “This maintenance activity has been closed. 
  There were problems, and all tasks were NOT completed successfully. See OpenMAINT for additional details.”
* If the Outcome is “Maintenance Not Required”, the comment will say “This maintenance activity has been closed. 
  It was determined this maintenance is not required. See OpenMAINT for additional details.”

|

.. figure:: /_static/skipped-comment.png
    :name: skipped-comment
    :width: 520 px

If the Group Leader decides to skip the next scheduled maintenance activity, the corresponding Jira ticket should be canceled, with a comment added saying “This scheduled maintenance activity has been skipped.”

|

.. figure:: /_static/update-due-dates-in-JIRA.png
    :name: update-due-dates-in-JIRA
    :width: 520 px

If the schedule of a maintenance activity is updated in OpenMAINT, the due date of the corresponding Jira ticket will be updated to match.
The comment added to the Jira ticket will depend on what changes were made to the schedule:

* If the cadence was maintained, a comment should be added that says “The due date was changed from [old due date] to [new due date].”
* If the maintenance activity schedule was paused, a  comment should be added that says “This activity has been paused until [restart date]. 
  It has been paused for this reason: [insert reason provided by Group Leader within OpenMAINT]”

|

.. figure:: /_static/CMMS-posts-comment-in-JIRA.png
    :name: CMMS-posts-comment-in-JIRA
    :width: 550 px

OpenMAINT will add comments to the Jira ticket throughout the workflow, when certain actions are taken within OpenMaint. 
In addition to the comments already mentioned that go along with specific actions taken by OpenMAINT, these include:

* If the assignee has been changed in OpenMAINT, make a comment saying “The OpenMAINT assignee has been changed from [old assignee] to [new assignee].”
* When the technician executes the maintenance activity, make a comment saying “The preventative maintenance activity has been executed.”
* If the preventative maintenance activity is suspended, make a comment saying “The preventative maintenance activity has been paused.”

|

.. figure:: /_static/Jira-schedule.png
    :name: Jira-schedule
    :width: 290 px

In the process of scheduling maintenance work, Group Leaders and Managers will move activities around in Jira (the primary place where resource scheduling occurs). 
We want the schedule in OpenMAINT to update to match. 
To facilitate this, OpenMAINT should maintain 2 dates for each maintenance activity:

* The original planned due date (i.e. the "ideal" maintenance due date, if activities followed the original schedule)
* The actual scheduled due date (which will match the original planned due date, unless things have been manually rescheduled)

Once per day, OpenMAINT will look at its tickets that are in the "Acceptance" or "Execute" stage, and check whether any of the corresponding tickets in Jira have been rescheduled. 
When doing this check, OpenMAINT will do the following:

* OpenMAINT will check the workflow status of the Jira ticket, and only consider tickets that are OPEN, IN PROGRESS, or CANCELLED. Any other status can be ignored.
* If the Jira ticket has been CANCELLED, OpenMAINT will cancel its ticket.
* For OPEN and IN PROGRESS tickets, OpenMAINT will compare its maintenance activity due date (the actual scheduled due date) to the Jira ticket due date. 
  If it doesn't match, OpenMAINT will adjust the date of its maintenance activity to match Jira. 

This is done on a per-activity basis, and should not impact the scheduling of any future maintenance activities in OpenMAINT. 
When any future activities are generated in OpenMAINT, they should be scheduled based on the "ideal" maintenance schedule (i.e. original scheduled maintenance dates), not on any adjustments made in Jira.

|

.. _CMMS-JIRA-Workflow-API-Features:

Features within OpenMAINT
=========================

.. figure:: /_static/execute.png
    :name: execute
    :width: 280 px

In the “Acceptance” stage of OpenMAINT, the technician ONLY has the option to “Execute” the preventative maintenance activity. 
The Group Leader is the only one with the power to reject and close. 
If the technician is busy or thinks they’re not the right person for the job, they work with the Group Leader to reschedule and/or choose a new assignee in Jira.

|

.. figure:: /_static/CMMS-ticket-review.png
    :name: CMMS-ticket-review
    :width: 280 px

The technician doesn’t have the option to conclude the activity, instead they have the option to Send for Review. 
When the technician sends the maintenance activity for review, they should be required to enter the outcome, and the completion date of the work.
It should be clear that this is the date that physical work was completed, so they don’t update it if they have to go back and add paperwork.
The technician has 3 options when selecting the Outcome: Maintenance Successful, Maintenance Not Completed, and Maintenance Not Required.

|

.. figure:: /_static/CMMS-ticket-review-for-closure.png
    :name: CMMS-ticket-review-for-closure
    :width: 475 px

After the OpenMAINT maintenance activity ticket has been sent for review, only the Group Leader should have edit access.

|

.. figure:: /_static/Group-Leader-approval-choice.png
    :name: Group-Leader-approval-choice
    :width: 475 px

After reviewing the completed maintenance activity, the Group Leader has the action options to Conclude Activity, Send Back (Add report), or Send Back (Rework). 
When sending a maintenance activity back, the Group Leader will be required to write a comment about what needs to be done. 
Both Send Back options open up edit access to the Technician again. 
The Send Back (Rework) option will delete the completion date and the original Outcome, but will preserve the completed checklist (in case only some steps need to be reworked).


|

.. figure:: /_static/CMMS-popup-window.png
    :name: CMMS-popup-window
    :width: 550 px

When the Group Leader closes the OpenMAINT ticket, a pop-up window should ask them how they want to adjust the schedule for the next maintenance activity.
The pop-up should include the date of the next scheduled maintenance, and the typical maintenance period of this activity.
They should be allowed to choose one of the following options:

* **Maintain Date** maintains the current schedule

  * No due dates are adjusted with this option.
  * Example: If the normal cadence is maintenance once a month and the next scheduled activity is 2 weeks after maintenance was last completed, the due date will still be in 2 weeks.

* **Maintain Cadence of Next** maintains the activity frequency and adjust the schedule for the next scheduled maintenance activity

  * When selecting this option, and the relevant maintenance activity has previously been manually rescheduled, the Group Leader will be asked to confirm if they want to change the schedule.
  * Due date for only the next maintenance activity on the schedule is updated to maintain the normal cadence of the maintenance activity (if the Group Leader confirms).
    This updates both the original due date and the actual scheduled due date, and pushes the updated due date to Jira.
  * Example: If the normal cadence is once a month, the next maintenance activity will be rescheduled to be due 1 month after the last maintenance activity was completed.

* **Maintain Cadence of All** maintains the activity frequency and adjust the schedule for all upcoming maintenance activities

  * When selecting this option, and any relevant maintenance activities have previously been manually rescheduled, the Group Leader will be asked to confirm if they want to reschedule manually-adjusted activities. 
    The Group Leader should also have the option to cancel this selection and choose a different option from the original popup.
  * Whether the Group Leader selects yes or no, the original scheduled due date will be updated for all future maintenance activities (this change happens in OpenMAINT only, and is not pushed to Jira).
  * If the Group Leader selects no, then only maintenance activities that have not been manually rescheduled will be updated to maintain cadence. 
    This updates the actual scheduled due date only for those activities, and pushes the new dates to Jira.
  * If the Group Leader selects yes, all maintenance activities will have their actual scheduled due dates updated, and the data will be pushed to Jira.
  * Example: If the normal cadence is once a month, the next maintenance activity will be rescheduled to be due 1 month after the last maintenance activity was completed.

* **Skip Next** cancels the next maintenance activity and maintains the rest of the schedule

  * The next maintenance activity is skipped, and the schedule for the remaining maintenance activities stays the same.

* **Pause** is selected if this activity won't be done for a while. This option reschedules the next maintenance activity based on the selected date.

  * The Group Leader will be prompted to select or enter a date when the maintenance activity will resume.
  * The Group Leader will be required to write a comment saying why the maintenance activity is being paused.

Note that several of these options will require OpenMAINT to leave comments in Jira describing what has been done. 
These comments are described earlier in this document.

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

