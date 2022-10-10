# Create order

## Create order API

Creates an order

`POST /api/v1/orders/`


### Headers

???+ info "Header parameters"

    `RequestId` *string*
    :    Idempotency key for request. See [idempotency] for more information.

    `Authorization` *string* **required**
    :    To make REST API calls, include the bearer token in this header with the `Bearer` authentication scheme. The value is `Bearer <Access-Token>`

    `Content-Type` *string* **required**
    :    The media type. Required for operations with a request body. The value is `application/<format>`, where format is `json`.


### Request

???+ info "Request body"

    `external_id` *string* **required**
    :    Unique identifier of your system. iumiCash will save this to this order, 
         so you can further identify iumiCash order with your order instance.

    `items` *list of [*item object*](#item)* **required**
    :    Order items in the order.


### Response

???+ success "Response"

    `id` *UUID* **unique**
    
    :    iumiCash identifier. Using to unique identify resourses (in this example, order etc.)

    `external_id` *string* **unique**

    :    Unique identifier of your system. 

    `created_at` *datetime*
    
    :    Created time of the order in ISO format.

    `updated_at` *datetime*
    
    :    Updated time of the order in ISO format.

    `status` *enum* 
    
    :    Order status. Can be one of follows:
    
         * `created`
         * `approved`
         * `captured`
         * `cancelled`
         * `completed`

    `items` *list of [*item object*](#item)*

    :    Order items in the order.

    `links` *list of [*HATEOAS links*](#hateoas)*
    
    :    An array of request-related HATEOAS links. For example, 
         to complete payer approval, use `approve` link to redirect the payer.


### Examples

???+ example "Examples"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
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

## Objects

Request and response objects


### item

???+ info "Description"

    `name` *string*
    :   Name of the order.
    
    `description` *string*
    :   Description of the order.
    
    `amount` [*object*](#amount)
    :   Amount object.
    
    `count` *integer*
    :   Count of this item.


### amount

???+ info "Description"

    `value` *Decimal*
    :   amount
    
    `currency_code` *enum*. See [available currencies](#currencies).
    :   Currency of item


### HATEOAS

???+ info "Description"

    `link` *URL string*
    :   The complete target URL. To make the related call, combine the method with this `link`.
    
    `rel` *enum*
    :   The link relation type.
        
        Possible values:
       
        * `self` - do `method` with current resource.
        * `approve`
        * `capture`
        * `cancel`

    `method` *enum*
    :   The HTTP method required to make the related call. 
        
        Possible values:
        
        * `GET`
        * `POST`


### Currencies

???+ info "Description"

    Possible values:

    * `SBD`
    * `NZD`


[idempotency]: ../idempotency.md
[client secret]: ../vendors/vendor_registration.md
