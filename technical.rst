Technical Concepts
==================

Low-bandwidth Connectivity
--------------------------

mobile4D transmits the lowest amount of data possible to save bandwidth costs. Interrupted connections must be expected and taken into account.

Concepts
^^^^^^^^
 * Pictures are scaled down to a reasonable minumum.
 * Information is sent in batches, to make sure at least the first batch makes it to the destination server.
 * Important information is sent first.
 * Compression

 **Implementation:** Compression is not implemented currently, neither for push nor data transmission.


**Note:** Checksums for checking if a report has changed remotely have not shown to be successful, see Andreas Kästner's bachelor thesis.

**See also:** `Offline Functionality`_

Offline Functionality
---------------------

mobile4D aims at being fully operational without network access. Network access should only be required when definitely needed (e.g., when transmitting new data). Information once received must be available throughout. Lacking network must never block functionality.

**Implementation**:
 * *Transaction Queuing*: Requests to the REST interface are stored in a queue when the client is offline. When the network comes back, the queue is being emptied.

 * *Local Storage*: Web pages and mobile client store all information locally and sync content *only* if needed. Information received by Push already populates the local database, even if full information is not available yet.


Battery Life First
------------------

mobile4D must not comsume more battery than needed. There is clearly a tradeoff between battery consumption and keeping data up to date. This may result in the app not being aware of the current location in time and displaying information for recent locations. If in doubt, and information loss is not critical, battery life comes first.

**Implementation:** The current implmentation does no scheduled pulls, thus does not wake up more than needed to maintain the push connection. If not manually initiated, the app never pulls and only relies on push messages. The location service, however, is updated frequently (every 5 min?) for debug reasons.


Server-Client Functionality
---------------------------

Internet connectivity to/from Laos can be incredibly bad to a point of broken connections. Thus, the mobile4D server is supposed to run on a local machine.


Push Messaging
--------------

Push messages are a central concept. Important data is pushed to the clients rather than being pulled by them. All notifications rely on Push.

**Implementation**: Push to mobile via MQTT. Alternatively, a downward compatible "Push via IMAP" concept (Info is sent out via mailing lists, mails are picked up as push messages) has been successfull tested (compare Frommberger & Schmid 2014, ISCRAM Asia paper), it is currently disabled though.

**See also:** `Offline Functionality`_, `All Info to the Right Desk`_


Independency from Service Providers
-----------------------------------

Relying on the big internet service providers bears risks:

 * Free services might not remain free
 * Services might be restricted in usage (Google Maps API interaction, GCM)
 * Services might be administratively blocked (Google Services like GCM are blocked in China, and might easily be blocked elsewhere)

mobile4D tries not to rely on such 3rd party services and use open solutions.

**Implementation:** Push notifications are implemented through MQTT. Maps are taken from Google Maps due to poor coverage of OSM in Laos. SMS through FrontlineSMS.

**See also:** `Push Messaging`_
