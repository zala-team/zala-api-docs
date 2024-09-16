# Service Variant

A Variant of the service that could be booked alone or together with other variants of the same service.

## Properties

| Property    | Explanation                                                                                                               |
|-------------|---------------------------------------------------------------------------------------------------------------------------|
| id          | The unique UUID identifier for the Service Variant.                                                                       |
| venueId     | [Venue](https://github.com/zala-team/zala-api-docs/blob/master/resources/venue.md) where this service belongs             |
| serviceId   | [Service](https://github.com/zala-team/zala-api-docs/blob/master/resources/service.md) where this service belongs         |
| name        | Displayable name of the service's variant                                                                                 |
| description | Displayable description of the service's variant                                                                          |
| exclusions  | An array of variants id that must not be booked with this variant                                                         |
| parameters  | An array of parameters to configure the variant. Could be `null` if the variant doesn't use this option.                  |
| active      | If this service variant was erased by the business. Only active services will be returned unless `?inactive=true` is sent |                                                                                                                        |                       |
| createdAt   | Date when the Service was created in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                             |
| updatedAt   | Date when the Service was last updated in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                        |

#### Parameter

| Property   | Explanation                                                   |
|------------|---------------------------------------------------------------|
| key        | Parameter unique identifier for the variant. Ex: `size`       |
| value      | A possible value for the key parameter. Ex `small` or `large` |
| valueLabel | Displayable value for the parameter. Ex: `Corto`              |


## Endpoints

### GET /services/{service-id}/variants

Receive a list of all Variant's Services.

| Parameter | Explanation                                                                   |
|-----------|-------------------------------------------------------------------------------|
| inactive  | If `true` will return services erased by the business. By default is `false`. |
| page      | Page to show                                                                  |
| size      | Amount of results per page                                                    |
| q         | Search Services by the given name                                             |

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
    "parameters": [{
      "key": "size",
      "value": "small",
      "valueLabel": "Corto"
    },{
      "key": "size",
      "value": "large",
      "valueLabel": "Largo"
    }],
    "active": true,
    "createdAt": "2013-01-01T05:12:51-03:00",
    "updatedAt": "2013-01-01T05:12:52-03:00"
  }
]
```

### GET /services/{variant-id}/variants/{id}

Receive a single service

#### GET /services/23987439-2a3d-462c-810b-f6b8e57d4964/variants/fa04d601-661c-4f4f-aec3-c181adda8652

`HTTP/1.1 200 OK`

```json
{
  "id": "fa04d601-661c-4f4f-aec3-c181adda8652",
  "venueId": "52e15a31-639a-41fe-ac7e-d41841934aaf",
  "serviceId": "23987439-2a3d-462c-810b-f6b8e57d4964",
  "name": "Acme Service Variant",
  "description": "A service that does everything and more",
  "exclusions": [],
  "active": true,
  "createdAt": "2013-01-01T05:12:51-03:00",
  "updatedAt": "2013-01-01T05:12:52-03:00"
}
```