# Authentication

## Authentication

iumiCash REST APIs use [OAuth 2.0](https://oauth.net/2/) access tokens to authenticate requests. Your access token authorizes you to use the iumiCash REST API servers.

To call a REST API in your integration, you must exchange your **client ID** and **client secret** for an access token.  
You can find your client ID and client secret by logging in to iumiCash Developer Dashboard or somehow (we can send them to your email etc.)


## cURL

Example request to exchange your **client_id** and **client_secret** for **access_token**

```bash
curl -v -X POST "https://api-m.sandbox.paypal.com/v1/oauth2/token" \
    -u "<CLIENT_ID>:<CLIENT_SECRET>" \
    -H "Content-Type: application/x-www-form-urlencoded" \
    -d "grant_type=client_credentials"  
```
