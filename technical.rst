Technical Concepts
==================

Low-bandwidth Connectivity
--------------------------

mobile4D transmits the lowest amount of data possible to save bandwidth costs. Interrupted connections must be expected and taken into account.

**Implementation:**
 * Pictures are scaled down to a reasonable minumum.
 * Information is sent in batches, to make sure at least the first batch makes it to the destination server.
 * Important information is sent first.

**Note:** Checksums have not shown to be successful, see Andreas Kästner's bachelor thesis.

**See also:** `Offline Functionality`_

Offline Functionality
---------------------

mobile4D aims at being fully operational without network access. Network access should only be required when definitely needed (e.g., when transmitting new data). Information once received must be available throughout. Lacking network must never block functionality.

**Implementation**:
 * *Transaction Queuing*: Requests to the REST interface are stored in a queue when the client is offline. When the network comes back, the queue is being emptied.

 * *Local Storage*: Web pages and mobile client store all information locally and sync content *only* if needed. Information received by Push already populates the local database, even if full information is not available yet.
