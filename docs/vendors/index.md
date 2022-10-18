# Introduction to vendors

A `Vendor` is a third-party system integrated with iumiCash for the purpose of using iumiCash as one of the payment methods.


The following are the major steps required to integrate with iumiCash and start use iumiCash as a payment method:

* Apply to a iumiCash representative to get registered as a vendor. 

* Obtain `client_id`, `client_secret` credentials and API URL's from iumiCash.

* Get whitelisted by iumiCash to access its sandbox API instance.

* Test integration by exchanging data and queries with iumiCash sandbox instance.

* *(Optional)* Request iumiCash to apply `Verified` status to the vendor’s account.

* Get whitelisted by iumiCash to access its production API instance.

* Start creating orders as per the flow diagram. See [One-time payment][one_time_diagrams] for more information.

[one_time_diagrams]: ../diagrams/onetime.md
[recurrent_diagram]: ../diagrams/recurrent.md
