UI Concepts
===========

Usability guidelines
--------------------

One item per page
^^^^^^^^^^^^^^^^^
As it turns out that people tend to be challenged when being prompted with more than one question at once, mobile4D implements a "one question per view" concept. Next view, next question. Always be transparent on how many views/pages are still to come, though.

Allow for corrections anytime
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Make it easy to jump back and forth to any item in the reporting process to change things later. Show a summary in the end before submitting.


Avoid typing
^^^^^^^^^^^^
A design concept of mobile4D is to target ground level users with potentially low literacy. Thus, interfaces should avoid text where ever possible.

Text-free (or sparse-text) interfaces are targeted for the mobile client in the first place, not really the administrative website.

**Implementation:** Text-free interfaces have not really been investigated and implemented besides the well-known examples (flood level picker, wind picker for fires). Currently, mobile4D is everything but text-free.


Avoid numbers
^^^^^^^^^^^^^
Numbers, if crowdsourced, are rarely correct, and rather estimates. mobile4D tries to avoid people to have to give exact numbers, but rather used text-free interfaces, such as the flood-height picker.


Exploit prior knowledge
^^^^^^^^^^^^^^^^^^^^^^^
Already gathered knowledge should be used to help system use. Example: "popular" diseases should appear in a dropdown menu to not have people typing "malaria" each time again.

**Implementation:** The app uses autocomplete over already entered fields. Actually, this is not implemented for any disaster type. (Autocomplete is not great anyway.)



Transparency on data quality
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Always give the user feedback about the state and quality of the data shown. The user should be aware that data could be incomplete or not updated yet. Also, the system should always be transparent on which data has already been sent and which is still in the queue (without bothering the user too much).


 **Implementation:** Transparency on the queue is not implemented.



Chose the right input mode at the right time
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This should go without saying, but experience shows it is not: always choose appropriate input modalities. If only numbers are required, give a numberpad. Give email address keyboards (with @) for email addresses, etc.
