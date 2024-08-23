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
   Need to determine correct location for workflow diagram and include link in this document for access.
   Need to know if MagicDraw will assign an ID number and be the offical repository, or if DocuShare will need to be the official repository.
   If MagicDraw is the repository, then a reference copy will be included in this document's repository.

The version of this document corresponds to Version 1 of the workflow diagram.
The screenshots within this docushare are for reference.

.. See (link) for the workflow.


.. _CMMS-JIRA-Workflow-API-Actions:

Actions taken by OpenMAINT in JIRA
==================================

This interaction is one-way only.
OpenMAINT feeds info to JIRA, no info flows back.

.. figure:: /_static/open-jira-ticket.png
    :name: open-jira-ticket

Initial opening of the JIRA ticket.
This occurs one month before the schedule maintenance time in OpenMAINT to allow for resource scheduling and activity coordination in Jira.
The initial info transferred to the JIRA ticket should include:

* Assignee (this is the group lead for the assigned group, or the default manager (Eduardo) if that person doesn’t exist in JIRA).
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

..note::
   I recommend we generate a document or confluence page with a quick reference guide for what order to do things in when executing a preventive maintenance activity.
   We want people trained before they really start using it, but guides are super useful when first using a new system/workflow and can be a huge quality of life improvement.

Re-assigning the JIRA ticket if the Team is changed in OpenMAINT (ticket is assigned to the group lead or the default manager).
The Team should only be changeable by the Group Leader.

.. figure:: /_static/CMMS-reassigns-jira-ticket.png
    :name: CMMS-reassigns-jira-ticket

If a maintenance activity gets rejected and closed, OpenMAINT reassigns the Jira ticket to the Group Leader and adds a comment saying, “the preventative maintenance activity was rejected by the assignee.”

.. figure:: /_static/CMMS-reassigns-JIRA-ticket-with-comment.png
    :name: CMMS-reassigns-JIRA-ticket-with-comment

When the technician has finished updating the preventative maintenance activity and sends it for review, OpenMAINT will transition the Jira ticket from “IN PROGRESS” to “IN REVIEW”.
It will also add the group leader as the reviewer.
It will also make a comment saying,
“The maintenance activity has been completed.
The outcome was [Outcome].
See OpenMAINT for additional details.”

.. note::
   Rubin Team: We need to look at whatever other fields are required in JIRA to transition maintenance tickets in Jira from In Progress to In Review.
   Then make sure those details are filled by OpenMAINT so that the transition doesn’t fail.

.. note::
   There’s the option to add comments to the outcome.
   We can either have them repeated here or make anyone who wants those details go to OpenMAINT.
   We could also consider having the Group Leader write up a little summary of how things went when they do closeout, instead of having the tech deal with that.
   This is for the purposes of management oversight so they don’t have to worry about access to OpenMAINT.

.. figure:: /_static/CMMS-changes-Jira-status-review.png
    :name: CMMS-changes-Jira-status-review

If the Group Leader changes the OpenMAINT ticket back to in progress (i.e., takes it out of review and sends it back to the technician for additional work), OpenMAINT will transition the Jira ticket from “IN REVIEW” to “IN PROGRESS”.

.. figure:: /_static/CMMS-changes-Jira-status-progress.png
    :name: CMMS-changes-Jira-status-progress

If the Group Leader closes the OpenMAINT ticket, OpenMAINT will automatically change the status of the Jira ticket to “CLOSED”.

.. figure:: /_static/CMMS-changes-Jira-status-closed.png
    :name: CMMS-changes-Jira-status-closed

If the schedule of a maintenance activity is updated in OpenMAINT, the due date of the corresponding Jira ticket will be updated to match.
A comment should be added that says “The due date was changed from [old due date] to [new due date].”

.. figure:: /_static/update-due-dates-in-JIRA.png
    :name: update-due-dates-in-JIRA

OpenMAINT will add comments to the Jira ticket throughout the workflow, when certain actions are taken within OpenMaint.
In addition to the comments already mentioned that go along with specific actions taken by OpenMAINT, these include:

* If the assignee has been changed in OpenMAINT, make a comment saying “The OpenMAINT assignee has been changed from [old assignee] to [new assignee].”
* When the technician executes the maintenance activity, make a comment saying “The preventative maintenance activity has been executed.”
* If the preventative maintenance activity is suspended, make a comment saying “The preventative maintenance activity has been paused.”

.. figure:: /_static/CMMS-posts-comment-in-JIRA.png
    :name: CMMS-posts-comment-in-JIRA

If the Group Leader decides to skip the next scheduled maintenance activity, the corresponding Jira ticket should be canceled, with a comment added saying “This scheduled maintenance activity has been skipped.”

.. note::
   We need to find out from TecnoTeca what their system does when you skip a preventative maintenance activity. Do they keep a record of it being skipped?

.. figure:: /_static/skipped-comment.png
    :name: skipped-comment


.. _CMMS-JIRA-Workflow-API-Features:

Features within OpenMAINT
=========================

In the “Acceptance” stage of OpenMAINT, the technician either has the option to “Execute” the preventative maintenance activity, or “Reject and Return to Group Leader”.

.. figure:: /_static/reject-or-execute.png
    :name: reject-or-execute

When the technician sends the maintenance activity for review, they should be required to enter the completion date of the work.
It should be clear that this is the date that physical work was completed, so they don’t update it if they have to go back and add paperwork.

.. note::
   I just realized that one thing missing from the workflow is some check for what date the work was completed. We could use the date that the workflow is sent to review, but that’s no longer correct if it gets sent back and only documentation needs to be added. But if we don’t require a date update, there’s always a change that the tech forgets to update it. Maybe when the group leader sends it back we actually have them specify within OpenMAINT whether it’s for documentation or for rework, and if it’s for rework they’re required to update the completion date, but they’re blocked from updating it if it’s for documentation?

.. figure:: /_static/CMMS-ticket-review.png
    :name: CMMS-ticket-review

After the OpenMAINT maintenance activity ticket has been sent for review, only the Group Leader should have edit access.

.. figure:: /_static/CMMS-ticket-review-for-closure.png
    :name: CMMS-ticket-review-for-closure

When the Group Leader closes the OpenMAINT ticket, a pop-up window should ask them how they want to adjust the schedule for the next maintenance activity.
The pop-up should include the date of the next scheduled maintenance, and the typical maintenance period of this activity.
They should be allowed to choose one of the following options:

* Maintain the current schedule

  * No due dates are adjusted with this option.
  * If the normal cadence is maintenance once a month and the next scheduled activity is 2 weeks after maintenance was last completed, the due date will still be in 2 weeks.

* Skip the next maintenance activity and maintain the rest of the schedule

  * (NOTE: This will override any schedule changes in Jira)
  * The next maintenance activity is skipped, and the schedule for the remaining maintenance activities stays the same.

* Maintain the activity frequency and adjust the schedule

  * Due dates for all future maintenance activities on the schedule are updated to maintain the normal cadence of the maintenance activity.

If the normal cadence is once a month, the next maintenance activity will be rescheduled to be due 1 month after the last maintenance activity was completed.

.. figure:: /_static/CMMS-popup-window.png
    :name: CMMS-popup-window
