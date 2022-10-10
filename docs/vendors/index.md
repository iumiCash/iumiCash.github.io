# Vendors - Introduction

`Vendor` is a system actor who aimed to integrate iumiCash as a payment method for any value of its system.

Instructions on how to implement iumiCash as a payment method:

* Get `client_id`, `client_secret` from iumiCash. There are two ways to get it:
    * Register own application in iumiCash system. See more information in [*Registration page*](vendor_registration.md).
    * Contact with *andrew.taylor@iumi.cash* for providing `client_id`, `client_secret`.


* *(Optional)* Request iumiCash to provide `Verified` status of your account.

* To create order, see flow diagrams: [One-Time Payment][one_time_diagrams] / [Recurrent Payment][recurrent_diagram].

[one_time_diagrams]: ../diagrams/onetime.md
[recurrent_diagram]: ../diagrams/recurrent.md
