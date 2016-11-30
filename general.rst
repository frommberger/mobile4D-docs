
General Concepts
================

Disaster Information
--------------------

Some data fields are present for any disaster information type. Those are:


Date/time of incident
  Timestamp when the incident *occured*
Date/time of report
  Timestamp when the report was *issued*
Location (point)
  A point-based geolocation
Location (polygons) (several, optional)
  A set of polygons to further define the incident area
Image Files (several, optional)
  Files as JPG or PNG
PDF Documents (several, optional)
  PDF documents (meant as tutorial information mainly)
Additional information (free text, optional)
  The usual *anything you would like to add*
Reporter
  Name and contact info of the reporter
Severity, Urgency, Certainty
  see below: `Severity, Urgency, Certainty`_
Contact telephone number
  can be different from reporter’s number

Every disaster report has an individual unique URI.

**Note:** A subset of the data fields correspond to the Common Alerting Protocal (CAP) data fields. Some fields are additional, some CAP fields are not covered ("certainty", e.g., is completely omitted). However, CAP import/export is possible.


Severity, Urgency, Certainty
----------------------------
Severity, Urgency, and Certainty are different concepts. All of them relate to data fields in the CAP standards.

Severity
  indicates how severe an incident is (a light or a heavy flooding)
Urgency
  indicates how urgent it is to react on the incident
Certainty
  indicates a confidence level, how certain the reporter is that the even has happened as described

All experience shows that people are having a hard time to distinguish between the concepty (compare to issue trackers - same thing). Severity and urgency are strongly correlated anyway, and people tend to always put the highest urgency for their own case. Certainty basically is rather a measure for the reporter's self esteem than a quality measure for the report.

**Implementation:** Usually, only one value for urgency and severity is asked for, with a traffic light concept (green/red/yellow). Both fields are set to the same value. Certainty is completely omitted.

**Bugs:** Setting urgency/severity from the web frontend has been accidently forgotten in the current implementation.

Reports
-------

In general, a report consists of the initial disaster information set and a *sequence of updates*. The initial report is a structured data set with a set of fixed questions and data fields. Any update consists of a block of free text and, optionally, changes for the data fields of the initial disaster information set. Every report is personalized to a reporter. Thus, a report is a conversation with updated values. Generally, the values of the most recent update are considered to be the actual values.

**Note:**
Due to misunderstandings, report updates might be called ”notifications” in some part of code, apps and documentation

**Implementation:**
Updating disaster information sets from the mobile app is currently not possible. Adding comments and media is, but not changing values.

**See also:** `Disaster Information`_


Current Location and Home Location
----------------------------------

Generally, every user has two location properties: *current location* and *home location*. While current location is determined from the position of the mobile device, home location is a property that can be set within the app or web settings.

The idea of home location is to have the user updated about a fixed location of his interest without him having to worry about manual subscriptions. The assumption is that everybody would care about his/her home locattion.


**Implementation:** Current location is updated in fixed intervals in the mobile app. High precision is not needed. Basically, an update triggers an updated subscription to a notification topic. Current update frequency is very high, but should be lowered in productive use.

**Bugs:** Changing the home location in the web interface is not synced to the mobile client. Apparently, there is no syncing mechanism for user data yet.

.. todo:: Check this bug


Subscription
------------

Reports can be subscribed to ("starring"). If a user subscribes to a report, he/she will get notifications about this report regardless of his/her locations.

**Bugs:** Starred reports are not synchronized between mobile and web client. Apparently, there is no syncing mechanism for user data yet.


Verification
------------

mobile4D supports multiple verifications. In general, we have two kinds of verification:

1. *Administrative Verification* A verification of a report given by an administrative user (district, province, MAF). Credibility is given by the verifying user’s rank.

2. *Crowd Verification* Verification by anybody. Usually, the number of verifications indicates credibility (the more, the more reliable).

To account for both types of verifications, mobile4D tracks every single verification. In general, every user can verify any report.

**Current implementation:** The number of verifications and the highest administrative verification rank are shown. A push notification on "verified" is possible, but currenly disabled. Crowd verification is not implemented yet.

**Note:** As mobile4D has not been rolled out to non-administrative users yet, verification did not play a greater role in the current deployment.

**See also:** `Acknowledgement / Seen`_


Acknowledgement / Seen
------------------------

mobile4D reports have a field for being ”seen”, that is, for being acknowledged. A report is auto- matically marked seen as soon as a administrative user (district or higher) has read the report.

**Implementation:** A push notification on "seen" is possible, but currenly disabled.

**See also:** `Verification`_


Responsible Persons
-------------------

Any administrative user has the possibility to assign himself/herself as "responsible" for a report from the web frontend. In this case, his/her contact information is prominently displayed.

**Implementation**: Responsible persons are not shown in the mobile app.

**Note:** Responsible persons for a disaster should not be confused with `Officials`_ (which are responsible for an administrative entity).

.. todo:: Verify the missing Android implementation



Officials
---------

If a user is assigned a department (via the web frontend), he/she is regarded as an "offcial" for this entity (district/province). Offcials are shown prominently based on location (or home location).

**Implementation:** Officials are not shown in the web frontend (besides administrative area). In the mobile app, officials are a top level menu entry, showing names, affiliation,

**See also:** `Responsible Persons`_+


User Roles
----------

User roles define which permissions a user has. The mobile4D system aims at being not fine-grained to avoid permission problems and testing overhead. Basically, there are only three types of user roles, which are bound to the administrative level of the user.

1. Guests, who can only read/write
2. Logged in users, who are also able to send and update disaster reports
3. Users with an administrative authority (district level upwards) who can perform administrative tasks on reports (closing, merging, assigning responsibilities, etc.)

In addition, there is also an "administrator" user role, that is not bound to an administrative level and allows administration of the user database.

**Implementation:** In addition to "administrator", the roles "disaster coordinator", "mobile4D", and "smsauthority" are defined. It is dubious what their role is.

.. todo:: Check user roles




Tutorials
---------

Disaster specific tutorials are simply PDF files that can be attached to any disaster report. In addition to that, the mobile4D app has a section for "Tutorials" that are meant as some general download section and simply points to a HTTP resource offering PDF files.

**Implementation:** When PRAM KSN was still up and running, the app pointed to the PRAM KSN download section. As of now, the section is empty.


Outlets and interfaces
----------------------

mobile4D supports several outlet channels:

 * Push notifications (to mobile client and website)
 * RSS feed
 * Twitter feed
 * CAP feed (Common Alerting Protocol)
 * SMS
 * Email

CAP, as an ISO standard, is meant to provide an interchange format to other systems and interfaces.

**Implementation:** Push is implemented through MQTT (moquette), SMS uses FrontlineSMS. SMS is currently disabled (it used Michael's private phone). Facebook outlet could be coupled to Twitter (however, not fully reliable, especially not real-time).


All Info to the Right Desk
--------------------------

mobile4D wants to make sure that *all* relevant people receive notifications about disaster reports and updates. Those are:

 * People with current or home location in the disaster scope area
 * The responsible people on district level
 * The responsible people on province level (to be discussed)
 * People who subscribed to the disaster report (see `Subscription`_)
