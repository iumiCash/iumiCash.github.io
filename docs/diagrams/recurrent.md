# Recurrent payment flow


Recurrent payments works like a [one-time payment], except that 
recurrent charging period made by vendor itself via user's [access token] and [refresh token].

!!! note
    Refresh tokens has infinite duration (see [refresh token]). So the vendor can
    make requests without user's action. Except that user can constraint vendor's 
    permissions. See [failed requests] for more information.

[one-time payment]: ../diagrams/onetime.md
[access token]: ../authentication/token.md#authorization-code
[refresh token]: ../authentication/token.md#refresh-token
[failed requests]: ../responses.md#failed-requests
