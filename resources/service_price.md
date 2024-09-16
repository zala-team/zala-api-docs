# Service Price

A Price can be the current or future value of a service when a service is booked in Zala's business.

### GET /services/{service-id}/prices

Receive the list of scheduled prices for a specific service

## Properties

| Property              | Explanation                                                                                                                                                                                                 |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id                    | The unique UUID identifier for the Service's Price. Will be `current` if is the current start price or a UUID identifier if is a future price.                                                              |
| serviceId             | [Service](https://github.com/zala-team/zala-api-docs/blob/master/resources/service.md) owner of this Price                                                                                                  |
| price                 | Base price of the service in Money format. `null` if the service use variants                                                                                                                               |
| firstBookingEnabled   | If the service has an additional fee for the first booking of the [Customer](https://github.com/zala-team/zala-api-docs/blob/master/resources/customer.md). `true` or `false`                               |
| firstBookingFeeAmount | Price of the additional fee of the service in Money format.                                                                                                                                                 |
| downPaymentAmount     | Amount required to be paid upfront to the booking date in Money format. `null` if there is no up-front fee                                                                                                  |
| variantPrices         | [Variant Price](#variant-price) array of prices of all the [Service Variants](https://github.com/zala-team/zala-api-docs/blob/master/resources/service_variant.md). `Null` if service doesn't use variants. |
| createdAt             | Date when the Order was created in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                                                                                                                 |
| updatedAt             | Date when the Order was last updated in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                                                                                                            |

#### Variant Price

| Property   | Explanation                                                                                                                                |
|------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| id         | The unique UUID identifier for the [Service Variants](https://github.com/zala-team/zala-api-docs/blob/master/resources/service_variant.md) |
| price      | Base price of the service's variant in Money format. `null` if the service use variants                                                    |
| parameters | If the service has parameters will contain an array with the price of each parameter. [Variant Parameter Price](#variant-parameter-price)  |

#### Variant Parameter Price

| Property | Explanation                                                   |
|----------|---------------------------------------------------------------|
| key      | Parameter unique identifier for the variant. Ex: `size`       |
| value    | A possible value for the key parameter. Ex `small` or `large` |
| price    | Price of the additional fee of the service in Money format.   |

## Endpoints

### GET /services/{service-id}/prices

Receive a list of all (current or future) Services prices.

| Parameter   | Explanation                                                                                                         |
|-------------|---------------------------------------------------------------------------------------------------------------------|
| currentOnly | If `true` will return just the current price. By default is `false` and all current and future prices are returned. |
| startsAt    | Show Prices that start after the given date (inclusive) [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)    |
| page        | Page to show                                                                                                        |
| size        | Amount of results per page                                                                                          |

#### GET /services/23987439-2a3d-462c-810b-f6b8e57d4964/variants

`HTTP/1.1 200 OK`

```json
[
  {
    "id": "fa04d601-661c-4f4f-aec3-c181adda8652",
    "venueId": "52e15a31-639a-41fe-ac7e-d41841934aaf",
    "serviceId": "23987439-2a3d-462c-810b-f6b8e57d4964",
    "name": "Acme Service Variant",
    "description": "A service that does everything and more",
    "exclusions": [],
    "parameters": [
      {
        "key": "size",
        "value": "small",
        "valueLabel": "Corto"
      },
      {
        "key": "size",
        "value": "large",
        "valueLabel": "Largo"
      }
    ],
    "active": true,
    "createdAt": "2013-01-01T05:12:51-03:00",
    "updatedAt": "2013-01-01T05:12:52-03:00"
  }
]
```
