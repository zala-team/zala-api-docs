# Service

A Service can be reserved in Zala's business.

## Properties

| Property              | Explanation                                                                                                                                                      |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id                    | The unique UUID identifier for the Service.                                                                                                                      |
| venueId               | [Venue](https://github.com/zala-team/zala-api-docs/blob/master/resources/venue.md) where this service belongs                                                    |
| name                  | Displayable name of the service                                                                                                                                  |
| description           | A friendly description of what this services includes                                                                                                            |
| imageUrl              | The URL where the main image is stored                                                                                                                           |
| link                  | The Zala's URL link to book this service                                                                                                                         |
| locationType          | How this service is done. Values could be "REMOTE", "AT_ADDRESS" or "CUSTOMER_LOCATION"                                                                          |
| pricingType           | If the business will charge or not for this service. "FREE" or "PAID" only.                                                                                      |
| minBookableDays       | The minimum amount of days that need in advance to book this service. Starts at 0.                                                                               |
| maxBookableDays       | The maximum amount of days in advance that this service could be booked. Starts at 1.                                                                            |
| selectionType         | If the service will or not include [Variants](https://github.com/zala-team/zala-api-docs/blob/master/resources/service_variant.md). "SIMPLE" or "VARIANTS" only. |
| allowedPaymentMethods | An array of the supported payment types for this service. Values could be "AT_THE_PROPERTY", "ONLINE_CHECKOUT", "ONLINE_PARTIAL_CHECKOUT"                        |                       |
| durationInMinutes     | Amount of time that required to be spend to execute the service                                                                                                  |                       |
| beforeBufferMinutes   | Amount of time to flag as booked before the service. Which is generally used for travel or set up.                                                               |                       |
| afterBufferMinutes    | Amount of time to flag as booked before the service. Which is generally used for travel or clean up.                                                             |                       |
| active                | If this service was erased by the business. Only active services will be returned unless `?inactive=true` is sent                                                |                       |                                                                                                              |                       |
| createdAt             | Date when the Service was created in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                                                                    |
| updatedAt             | Date when the Service was last updated in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                                                               |

## Endpoints

### GET /services

Receive a list of all Services.

| Parameter     | Explanation                                                                                                                  |
|---------------|------------------------------------------------------------------------------------------------------------------------------|
| locationType  | Show Services with a given location type. Possible values are "ANY" (default), "REMOTE", "AT_ADDRESS" or "CUSTOMER_LOCATION" |
| pricingType   | Show Services with a given pricing type. Possible values are "ANY" (default), "FREE", "PAID"                                 |
| selectionType | Show Services with a given selection type. Possible values are "ANY" (default), "SIMPLE", "VARIANTS"                         |
| inactive      | If `true` will return services erased by the business. By default is `false`.                                                |
| page          | Page to show                                                                                                                 |
| size          | Amount of results per page                                                                                                   |
| q             | Search Services by the given name                                                                                            |

#### GET /services

`HTTP/1.1 200 OK`

```json
[
  {
    "id": "23987439-2a3d-462c-810b-f6b8e57d4964",
    "venueId": "52e15a31-639a-41fe-ac7e-d41841934aaf",
    "name": "Acme Service",
    "description": "A service that does everything",
    "imageUrl": "https://d26lpennugtm8s.cloudfront.net/stores/046/themes/common/logo-ff622335866ee56df3bceed2e9d41469.png",
    "link": "https://reservas.zala.app/acme-business/acme-service",
    "locationType": "AT_ADDRESS",
    "pricingType": "PAID",
    "minBookableDays": 0,
    "maxBookableDays": 30,
    "selectionType": "VARIANTS",
    "allowedPaymentMethods": ["AT_THE_PROPERTY", "ONLINE_CHECKOUT"],
    "durationInMinutes": 60,
    "beforeBufferMinutes": 15,
    "afterBufferMinutes": 15,
    "active": true,
    "createdAt": "2013-01-01T05:12:51-03:00",
    "updatedAt": "2013-01-01T05:12:52-03:00"
  }
]
```

### GET /services/{id}

Receive a single service

#### GET /services/23987439-2a3d-462c-810b-f6b8e57d4964

`HTTP/1.1 200 OK`

```json
{
"id": "23987439-2a3d-462c-810b-f6b8e57d4964",
"venueId": "52e15a31-639a-41fe-ac7e-d41841934aaf",
"name": "Acme Service",
"description": "A service that does everything",
"imageUrl": "https://d26lpennugtm8s.cloudfront.net/stores/046/themes/common/logo-ff622335866ee56df3bceed2e9d41469.png",
"link": "https://reservas.zala.app/acme-business/acme-service",
"locationType": "AT_ADDRESS",
"pricingType": "PAID",
"minBookableDays": 0,
"maxBookableDays": 30,
"selectionType": "VARIANTS",
"allowedPaymentMethods": ["AT_THE_PROPERTY", "ONLINE_CHECKOUT"],
"durationInMinutes": 60,
"beforeBufferMinutes": 15,
"afterBufferMinutes": 15,
"active": true,
"createdAt": "2013-01-01T05:12:51-03:00",
"updatedAt": "2013-01-01T05:12:52-03:00"
}
```