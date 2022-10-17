# Introduction to vendors

A `Vendor` system is the third-party system integrated with iumiCash for the purpose of payment processing on behalf of the end-user.

Instructions on how to implement iumiCash as a payment method:

* Get `client_id`, `client_secret` and the API URL from iumiCash by applying to a iumiCash representative.
    * Register own application in iumiCash system. See more information in [*Registration page*](vendor_registration.md).
    * Contact with *andrew.taylor@iumi.cash* for providing `client_id`, `client_secret`.


* *(Optional)* Request a iumiCash representative to apply `Verified` status to the vendor’s account.


* To create order, see flow diagrams: [One-Time Payment][one_time_diagrams] / [Recurrent Payment][recurrent_diagram].

[one_time_diagrams]: ../diagrams/onetime.md
[recurrent_diagram]: ../diagrams/recurrent.md
