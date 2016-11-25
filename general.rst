
General Concepts
================

Disaster Information
--------------------

Some data fields are present for any disaster information type. Those are:


* Date of initial report
* Location (point)
* Location (polygons) (several, optional)
* Image Files (several, optional)
* PDF Documents (several, optional)
* Additional information (free text, optional)
* Reporter information
* Severity and Urgency (different concepts, but mobile4d only asks for oneof them, as people usually can’t distinguish them anyway)
* Contact telephone number (can be different from reporter’s number)

**Implementation:** Setting urgency/severity from the web frontend has been accidently omitted in the current implementation.

**Note:** A subset of the data fields correspond to the Common Alerting Protocal (CAP) data fields. Some fields are additional, some CAP fields are not covered. However, CAP import/export is possible.


Reports
-------

In general, a report consists of the initial disaster information set and a *sequence of updates*. The initial report is a structured data set with a set of fixed questions and data fields. Any update consists of a block of free text and, optionally, changes for the data fields of the initial disaster information set. Every report is personalized to a reporter. Thus, a report is a conversation with updated values. Generally, the values of the most recent update are considered to be the actual values.

**Note:**
Due to misunderstandings, report updates might be called ”notifications” in some part of code, apps and documentation

**Implementation:**
Updating disaster information sets from the mobile app is currently not possible. Adding comments and media is, but not changing values.

**See also:** `Disaster Information`_


Verification
------------

mobile4D supports multiple verifications. In general, we have two kinds of verification:

1. *Administrative Verification* A verification of a report given by an administrative user (district, province, MAF). Credibility is given by the verifying user’s rank.

2. *Crowd Verification* Verification by anybody. Usually, the number of verifications indicates credibility (the more, the more reliable).

To account for both types of verifications, mobile4D tracks every single verification. In general, every user can verify any report.

**Current implementation:** The number of verifications and the highest administrative verification rank are shown. A push notification on "verified" is possible, but currenly disabled.

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

**Implementation:** Officials are not shown in the web frontend (besides administrative area).

**See also:** `Responsible Persons`_




Current Location and Home Location
----------------------------------

Generally, every user has two location properties: *current location* and *home location*. While current location is determined from the position of the mobile device, home location is a property that can be set within the app or web settings.

The idea of home location is to have the user updated about a fixed location of his interest without him having to worry about manual subscriptions. The assumption is that everybody would care about his/her home locattion.


**Implementation:** Current location is updated in fixed intervals in the mobile app. High precision is not needed. Basically, an update triggers an updated subscription to a notification topic. Current update frequency is very high, but should be lowered in productive use.

**Bugs:** Changing the home location in the web interface is not synced to the mobile client. Apparently, there is no syncing mechanism for user data yet.


Tutorials
---------

Disaster specific tutorials are simply PDF files that can be attached to any disaster report. In addition to that, the mobile4D app has a section for "Tutorials" that are meant as some general download section and simply points to a HTTP resource offering PDF files.

**Implementation:** When PRAM KSN was still up and running, the app pointed to the PRAM KSN download section. As of now, the section is empty.
