# Order

An order is created when a customer completes the checkout process.

#### Table of Contents

> [Get all orders](#GET-orders)
>
> [Get an order](#GET-ordersid)
>
> [Register an order payment](#POST-ordersidpayments)
>
> [Cancel an order](#POST-ordersidcancel)

## Properties

| Property         | Explanation                                                                                                                                                                     |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id               | The unique numeric identifier for the Order.                                                                                                                                    |
| customer         | [Customer](https://github.com/zala-team/zala-api-docs/blob/master/resources/customer.md) that purchased this Order. Only given if the 'read_customers' scope is set for the app |
| status           | Order's status. Possible values are `PLACED`, `CANCELLED`, `PARTIALLY_PAID` or `PAID`                                                                                           |
| paymentType      | Type of required payment for the order. Possible values are `AT_THE_PROPERTY`, `ONLINE_CHECKOUT`,`ONLINE_PARTIAL_CHECKOUT`                                                      |
| bookings         | [Bookings](#Booking) All the details of the bookings associated to this order.                                                                                                  |
| payments         | [Payments](#Payment) All the details of the payment transactions that were recorded in this order.                                                                              |
| total            | Total price of the order in Money format.                                                                                                                                       |
| downPaymentTotal | Amount required to be paid upfront to the booking date in MoneyFormat.                                                                                                          |
| firstBookingFee  | Fee charged to the order for being a first booking.                                                                                                                             |
| createdAt        | Date when the Order was created in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                                                                                     |
| updatedAt        | Date when the Order was last updated in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                                                                                |

#### Booking

The `bookings` field has the following contents:

| Property          | Explanation                                                                                                                             |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| id                | Booking unique identifier (UUID)                                                                                                        |
| service           | [Service](https://github.com/zala-team/zala-api-docs/blob/master/resources/service.md) purchased                                        |
| status            | Status of the booking. Possible values are `PENDING`, `IN_PROGRESS`, `COMPLETED`, `NO_SHOW`, `CANCELLED_BY_USER`, `CANCELLED_BY_SELLER` |
| startsAt          | Date when the Booking will start in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601) using booking timezone                     |
| endsAt            | Date when the Booking will end in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601) using booking timezone                       |
| timezone          | Timezone of the Booking in IANA Format                                                                                                  |
| utcStartsAt       | Date when the Booking will start in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601) using UTC timezone                         |
| utcEndsAt         | Date when the Booking will end in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601) using UTC timezone                           |
| durationInMinutes | Numeric value of how much minutes the booking should last                                                                               |
| subtotal          | Value charged for this booking in Money Format without other fees                                                                       |
| timesRescheduled  | Numeric amount of times this particular booking was rescheduled                                                                         |
| createdAt         | Date when the Order was created in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                                             |
| updatedAt         | Date when the Order was last updated in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                                        |

#### Payment

The `payment` field has the following contents:

| Property     | Type    | Explanation                                                                                                                 |
|--------------|---------|-----------------------------------------------------------------------------------------------------------------------------|
| id           | String  | Unique UUID identifier of the Payment.                                                                                      |
| amount       | Money   | Amount paid or refunded.                                                                                                    |
| type         | String  | Show if this was a payment or a refund. Values could be `PAYMENT` or `REFUND`                                               |
| refunded     | Boolean | If payment was refunded the value will be `true`                                                                            |
| privateNote  | String  | If the business recorded this payment using manually triggers it could enter a description of the reason                    |
| providerType | String  | The provider who processed the payment Values could be `MERCADO_PAGO`, `MERCADO_PAGO_API`, `ZALA_BUSINESS`, `EXTERNAL_APP`  |
| providerId   | String  | If providerId is `EXTERNAL_APP` this field will contain the id of the app who may the payment. Otherwise will be `internal` |
| createdAt    | String  | Date when the Order was created in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                                 |

## Endpoints

### GET /orders

Receive a list of all Orders.

| Parameter    | Explanation                                                                                                                    |
|--------------|--------------------------------------------------------------------------------------------------------------------------------|
| status       | Show Orders with a given state. Possible values are `any` (empty default), `PLACED`, `CANCELLED` or `PAID` or `PARTIALLY_PAID` |
| customerIds  | Restrict results to the specified customer IDs (comma-separated)                                                               |
| serviceIds   | Restrict results to the specified services IDs (comma-separated)                                                               |
| createdAtMin | Show Orders created after date (inclusive) [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                            |
| createdAtMax | Show Orders created before date (exclusive) [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                           |
| updatedAtMin | Show Orders last updated after date (inclusive) [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                       |
| updatedAtMax | Show Orders last updated before date (exclusive) [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                      |
| page         | Page to show                                                                                                                   |
| size         | Amount of results per page                                                                                                     |
| sort         | Sort results by a given field. Possible values are `CREATED_AT_ASC` (default) or `CREATED_AT_DESC`                             |                            |

#### GET /orders

`HTTP/1.1 200 OK`

```json
[
  {
    "id": 124156,
    "customer": {
      "id": "d0906db4-af79-4144-bf0f-2dbede77a15b",
      "firstName": "John",
      "lastName": "Doe",
      "email": "john@doe.com.ar",
      "birthDate": "2013-08-01",
      "phone": "+549111111999",
      "identifier": null,
      "pets": [
        {
          "id": "7e5209fe-8a80-41b8-b1a7-9addfd938029",
          "name": "Scooby",
          "metadata": "{\"size\":\"MEDIUM\"}",
          "type": "DOG",
          "familyType": "ANIMAL",
          "createdAt": "2024-08-30T02:32:42"
        }
      ]
    },
    "status": "PLACED",
    "paymentType": "AT_THE_PROPERTY",
    "bookings": [
      {
        "id": "5583d0c8-68a1-48b3-b99a-fe1cc64c6f99",
        "service": {
          "id": "63cae9e3-44b0-4bd2-8fa5-e5962960aa7c",
          "venue": {
            "id": "52e15a31-639a-41fe-ac7e-d41841934aaf",
            "primary": true,
            "name": "Downtown",
            "photoUrl": null,
            "description": "",
            "address1": "Av. Siempre Viva 123",
            "address2": null,
            "city": "Downtown",
            "state": "Springfield",
            "zipCode": "1706",
            "active": true,
            "createdAt": "2024-06-30T19:26:46",
            "venueId": "52e15a31-639a-41fe-ac7e-d41841934aaf"
          },
          "venueId": "52e15a31-639a-41fe-ac7e-d41841934aaf",
          "name": "Acme Service",
          "description": null,
          "photoUrl": null,
          "link": "acme-service",
          "locationType": "AT_ADDRESS",
          "pricingType": "PAID",
          "downPaymentAmount": null,
          "selectionType": "SIMPLE",
          "downPaymentType": null,
          "allowedPaymentMethods": [
            "AT_THE_PROPERTY"
          ],
          "total": {
            "value": 2000.00,
            "currency": "ARS"
          },
          "firstBookingEnabled": false,
          "firstBookingFeeAmount": null,
          "status": "PUBLISHED",
          "listedInHome": true,
          "active": true,
          "createdAt": "2024-06-30T19:37:41"
        },
        "status": "PENDING",
        "startsAt": "2024-09-06T17:00:00",
        "timezone": "America/Argentina/Buenos_Aires",
        "durationInMinutes": 30,
        "subtotal": {
          "value": 20000.00,
          "currency": "ARS"
        },
        "timesRescheduled": 0,
        "endsAt": "2024-09-06T17:30:00",
        "utcStartsAt": "2024-09-06T20:00:00",
        "utcEndsAt": "2024-09-06T20:30:00",
        "createdAt": "2024-08-30T02:32:42",
        "updatedAt": "2024-08-30T02:32:42"
      }
    ],
    "payments": [],
    "total": {
      "value": 20000.00,
      "currency": "ARS"
    },
    "createdAt": "2024-08-30T02:32:42",
    "active": true,
    "guest": false,
    "firstBookingFee": null,
    "downPaymentTotal": null
  }
]
```

### GET /orders/{id}

Receive a single Order

| Parameter | Explanation                                               |
|-----------|-----------------------------------------------------------|
| fields    | Comma-separated list of fields to include in the response |

#### GET /orders/450789469

`HTTP/1.1 200 OK`

```json
{
  "id": 124156,
  "customer": {
    "id": "d0906db4-af79-4144-bf0f-2dbede77a15b",
    "firstName": "John",
    "lastName": "Doe",
    "email": "john@doe.com.ar",
    "birthDate": "2013-08-01",
    "phone": "+549111111999",
    "identifier": null,
    "pets": [
      {
        "id": "7e5209fe-8a80-41b8-b1a7-9addfd938029",
        "name": "Scooby",
        "metadata": "{\"size\":\"MEDIUM\"}",
        "type": "DOG",
        "familyType": "ANIMAL",
        "createdAt": "2024-08-30T02:32:42"
      }
    ]
  },
  "status": "PLACED",
  "paymentType": "AT_THE_PROPERTY",
  "bookings": [
    {
      "id": "5583d0c8-68a1-48b3-b99a-fe1cc64c6f99",
      "service": {
        "id": "63cae9e3-44b0-4bd2-8fa5-e5962960aa7c",
        "venue": {
          "id": "52e15a31-639a-41fe-ac7e-d41841934aaf",
          "primary": true,
          "name": "Downtown",
          "photoUrl": null,
          "description": "",
          "address1": "Av. Siempre Viva 123",
          "address2": null,
          "city": "Downtown",
          "state": "Springfield",
          "zipCode": "1706",
          "active": true,
          "createdAt": "2024-06-30T19:26:46",
          "venueId": "52e15a31-639a-41fe-ac7e-d41841934aaf"
        },
        "venueId": "52e15a31-639a-41fe-ac7e-d41841934aaf",
        "name": "Acme Service",
        "description": null,
        "photoUrl": null,
        "link": "acme-service",
        "locationType": "AT_ADDRESS",
        "pricingType": "PAID",
        "downPaymentAmount": null,
        "selectionType": "SIMPLE",
        "downPaymentType": null,
        "allowedPaymentMethods": [
          "AT_THE_PROPERTY"
        ],
        "total": {
          "value": 2000.00,
          "currency": "ARS"
        },
        "firstBookingEnabled": false,
        "firstBookingFeeAmount": null,
        "status": "PUBLISHED",
        "listedInHome": true,
        "active": true,
        "createdAt": "2024-06-30T19:37:41"
      },
      "status": "PENDING",
      "startsAt": "2024-09-06T17:00:00",
      "timezone": "America/Argentina/Buenos_Aires",
      "durationInMinutes": 30,
      "subtotal": {
        "value": 20000.00,
        "currency": "ARS"
      },
      "timesRescheduled": 0,
      "endsAt": "2024-09-06T17:30:00",
      "utcStartsAt": "2024-09-06T20:00:00",
      "utcEndsAt": "2024-09-06T20:30:00",
      "createdAt": "2024-08-30T02:32:42",
      "updatedAt": "2024-08-30T02:32:42"
    }
  ],
  "payments": [],
  "total": {
    "value": 20000.00,
    "currency": "ARS"
  },
  "createdAt": "2024-08-30T02:32:42",
  "active": true,
  "guest": false,
  "firstBookingFee": null,
  "downPaymentTotal": null
}
```

### POST /orders/{id}/cancel

Cancel a single Order only if none of their bookings have started.

| Parameter      | Explanation                                                                                        |
|----------------|----------------------------------------------------------------------------------------------------|
| notifyCustomer | Notify the customer of the cancellation of their bookings. Default value is `true`                 |
| refundCustomer | Attempt to refund customer for all charged amount using payment providers. Default value is `true` |

#### POST /orders/450789469/cancel

`HTTP/1.1 200 OK`

```json
{
  "id": 124156,
  "customer": {
    "id": "d0906db4-af79-4144-bf0f-2dbede77a15b",
    "firstName": "John",
    "lastName": "Doe",
    "email": "john@doe.com.ar",
    "birthDate": "2013-08-01",
    "phone": "+549111111999",
    "identifier": null,
    "pets": [
      {
        "id": "7e5209fe-8a80-41b8-b1a7-9addfd938029",
        "name": "Scooby",
        "metadata": "{\"size\":\"MEDIUM\"}",
        "type": "DOG",
        "familyType": "ANIMAL",
        "createdAt": "2024-08-30T02:32:42"
      }
    ]
  },
  "status": "CANCELLED",
  "paymentType": "AT_THE_PROPERTY",
  "bookings": [
    {
      "id": "5583d0c8-68a1-48b3-b99a-fe1cc64c6f99",
      "service": {
        "id": "63cae9e3-44b0-4bd2-8fa5-e5962960aa7c",
        "venue": {
          "id": "52e15a31-639a-41fe-ac7e-d41841934aaf",
          "primary": true,
          "name": "Downtown",
          "photoUrl": null,
          "description": "",
          "address1": "Av. Siempre Viva 123",
          "address2": null,
          "city": "Downtown",
          "state": "Springfield",
          "zipCode": "1706",
          "active": true,
          "createdAt": "2024-06-30T19:26:46",
          "venueId": "52e15a31-639a-41fe-ac7e-d41841934aaf"
        },
        "venueId": "52e15a31-639a-41fe-ac7e-d41841934aaf",
        "name": "Acme Service",
        "description": null,
        "photoUrl": null,
        "link": "acme-service",
        "locationType": "AT_ADDRESS",
        "pricingType": "PAID",
        "downPaymentAmount": null,
        "selectionType": "SIMPLE",
        "downPaymentType": null,
        "allowedPaymentMethods": [
          "AT_THE_PROPERTY"
        ],
        "total": {
          "value": 2000.00,
          "currency": "ARS"
        },
        "firstBookingEnabled": false,
        "firstBookingFeeAmount": null,
        "status": "PUBLISHED",
        "listedInHome": true,
        "active": true,
        "createdAt": "2024-06-30T19:37:41"
      },
      "status": "CANCELLED_BY_SELLER",
      "startsAt": "2024-09-06T17:00:00",
      "timezone": "America/Argentina/Buenos_Aires",
      "durationInMinutes": 30,
      "subtotal": {
        "value": 20000.00,
        "currency": "ARS"
      },
      "timesRescheduled": 0,
      "endsAt": "2024-09-06T17:30:00",
      "utcStartsAt": "2024-09-06T20:00:00",
      "utcEndsAt": "2024-09-06T20:30:00",
      "createdAt": "2024-08-30T02:32:42",
      "updatedAt": "2024-08-30T02:32:42"
    }
  ],
  "payments": [],
  "total": {
    "value": 20000.00,
    "currency": "ARS"
  },
  "createdAt": "2024-08-30T02:32:42",
  "active": true,
  "guest": false,
  "firstBookingFee": null,
  "downPaymentTotal": null
}
```