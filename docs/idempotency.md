# Idempotency

You can make idempotent calls any number of times without concern that the server creates
 or completes an action on a resource more than once. You can retry idempotent calls that 
 fail with network timeouts or HTTP 5xx status codes for as long as the server stores the ID. 
 Idempotency enables you to correlate request payloads with response payloads, eliminate duplicate 
 requests, and retry failed requests or requests with unclear responses.

!!! success
    To enforce idempotency on REST API POST calls, use the `RequestId` request header, 
    which contains a unique user-generated ID that the server stores for a period of time.


!!! warning 
    For example, when you include a previously specified `RequestId` header in a request, 
    iumiCash returns the latest status of the previous request that used that same header. 
    Conversely, when you omit the `RequestId` header from a request, iumiCash duplicates the request.


??? example "Examples"

    === "Request"

        Example request with cURL.
        
        !!! Tip
            Here were are using `RequestId` idempotency key.

        ```bash hl_lines="4"
        curl -v -X POST https://api.iumi.cash/api/v1/orders/ \
        -H "Content-Type: application/json" \
        -H "Authorization: Bearer <Access-Token>" \
        -H "RequestId: 7b92603e-77ed-4896-8e78-5dea2050476a" \
        -d ' \
        {
          "external_id": "123456",
          "items": [
            {
              "name": "T-shirt",
              "description": "Brand new T-shirt",
              "amount": {
                "value": 100,
                "currency_code": "USD"
              },
              "count": 1
            }
          ]
        }
        '
        ```

    === "Response"

        A successful request returns the `HTTP 201 Created` status code and a JSON response body.

        ```bash
        {
          "id": "42481508-af81-43b9-82dd-d47d9e040ece",
          "external_id": "123456",
          "created_at": "2022-09-12T13:27:09.860466",
          "updated_at": "2022-09-12T13:27:09.860466",
          "status": "created",
          "client_id": "example_client_id",
          "items": [
            {
              "name": "T-shirt",
              "description": "Brand new T-shirt",
              "amount": {
                "value": 100,
                "currency_code": "USD"
              },
              "count": 1
            }
          ],
          "links": [
            {
              "link": "https://api.iumi.cash/api/v1/orders/42481508-af81-43b9-82dd-d47d9e040ece/",
              "rel": "self",
              "method": "get"
            }
          ]
        }
        ```
