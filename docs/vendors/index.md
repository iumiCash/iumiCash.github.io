# Vendors - Introduction

`Vendor` is a system actor who aimed to integrate iumiCash as a payment method for any value of its system.

Instructions on how to implement iumiCash as a payment method:

* Get `client_id`, `client_secret` from iumiCash. There are two ways to get it:
    * Register own application in iumiCash system. See more information in [*Registration page*](vendor_registration.md).
    * Contact with *andrew.taylor@iumi.cash* for providing `client_id`, `client_secret`.

* *(Optional)* Request iumiCash to provide `Verified` status of your account.

* [First Time]. If vendor does not have refresh token, then Req to /authorize, where the client have to auth in iumiCash system.

* /oauth/token . 

* Create order with this `access_token`. 

* Listen on `starlink.com/iumicash/order` notification from iumiCash about order status

* Your iumiCash balance has increased by order amount. Cash Out is manually procedure that you need to discuss with finance department.


