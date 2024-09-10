Venues
======

The Venue resource contains information about all Zala's business's venues.

Properties
----------

| Property    | Explanation                                                                                 |
|-------------|---------------------------------------------------------------------------------------------|
| id          | The unique identifier for the Venue (UUID)                                                  |
| name        | Business displayable name of the venue                                                      |
| description | A brief description of the business venue                                                   |
| primary     | Will be true if this is the main branch of the business                                     |
| photoUrl    | A picture representing the venue                                                            |
| address1    | Main street of the venue                                                                    |
| address2    | Optional information about the location of the venue                                        |
| city        | Venue's city                                                                                |
| state       | Venue's state. Like Buenos Aires or CABA                                                    |
| zipCode     | Venue's zipCode in string format                                                            |
| createdAt   | Date when the Venue was created in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601) |
| updatedAt   | Date when the Venue was updated in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601) |

Endpoints
---------

### GET /venues

Receive the list of Business's venues.

#### GET /venues

`HTTP/1.1 200 OK`

```json
[{
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
  "createdAt": "2024-06-30T19:26:46",
  "venueId": "52e15a31-639a-41fe-ac7e-d41841934aaf"
}]
```
