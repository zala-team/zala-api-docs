# Webhook

A Webhook is a tool that allows you to receive a notification for a certain event. It allows you to register an _https_
URL which will receive the event data, stored in JSON. Webhooks can be registered for the following events:

| Category | Events                         |
|----------|--------------------------------|
| App      | uninstalled/suspended/resumed  |
| Order    | created/updated/paid/cancelled |

To register for the order created event, for example, you should send `order/created` in the event field. Basically, you
make a POST request in Webhooks endpoint providing a body with the type of webhook you want to create as well as the URL
where you will be notified.

Now let's say you want to be notified whenever an order gets paid so you don't have to make scheduled calls to retrieve
this information. Here you should create a Webhook with the event `order/paid` and the URL where you will be notified in
your backend.

The `app/suspended` and `app/resumed` events refer to
the [suspension of API access due to lack of payment](https://github.com/zala-team/zala-api-docs/#suspension-of-api-access-due-to-lack-of-payment).

You are not allowed to use a localhost/zala domain for webhooks.

## Properties

| Property  | Explanation                                                                                        |
|-----------|----------------------------------------------------------------------------------------------------|
| id        | The unique numeric identifier for the Webhook                                                      |
| url       | The URL where the webhook should send the POST request when the event occurs. **Must be HTTPS**.   |
| event     | The event that will trigger the webhook                                                            |
| createdAt | Date when the Webhook was created in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)      |
| updatedAt | Date when the Webhook was last updated in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601) |

### Parameters

When doing the POST request, all webhooks will send the following parameters in JSON format:

- **business_id**: Store from where the event originated (received as `user_id` at
  the [authentication](./authentication.md) step)
- **event**: Event's name (order/created, order/updated, etc.)

Also, every webhook will send custom parameters, as follows:

#### app/uninstalled

- **id**: App's id.

#### order/created - order/updated - order/paid - order/cancelled

- **id**: Order's id.

#### Example webhook content

```json
{
  "business_id": "acme_business",
  "event": "order/created",
  "id": 1248601
}
```

### Retry policies

A Webhook notification expects a 2XX status code in response (to guarantee that the App has received the notification)
and has a timeout of 40 seconds. That is, we wait 40 seconds for a 2XX code in response (it can be any code that starts
with 2 - 200/201, etc).

When a webhook notification cannot be delivered due to a problem (e.g when the App is down) then retry policy is on.

The first 4 retries are: one immediately after timeout and then at approximately 5, 10 and 15 minutes. Then it starts to
do exponential backoff multiplying the waiting time of the previous one by 1.4 within the next 48 hours (up to 18
attempts).

Overview of the process:

- We send the webhook.
- We wait 40 seconds for response confirmation by the App.
- If we do not receive any responses, then retry policy is activated.

A success response code will stop next notifications. But, if that does not happen at all, notifications will be sent 18
times.

## Endpoints

### GET /webhooks

Receive a list of all Webhooks for your application.

| Parameter | Explanation                      |
|-----------|----------------------------------|
| event     | Show webhooks with a given event |
| page      | Page to show                     |
| size      | Amount of results per page       |

#### GET /webhooks

`HTTP/1.1 200 OK`

```json
[
  {
    "createdAt": "2013-01-03T09:11:51-03:00",
    "event": "app/uninstalled",
    "id": 101,
    "updatedAt": "2013-03-11T09:14:11-03:00",
    "url": "https://myapp.com/uninstall"
  },
  {
    "createdAt": "2013-04-07T09:11:51-03:00",
    "event": "order/created",
    "id": 5670,
    "updatedAt": "2013-04-08T11:11:51-03:00",
    "url": "https://myapp.com/order_created_hook"
  }
]
```

### GET /webhooks/{id}

Receive a single Webhook

#### GET /webhooks/5670

`HTTP/1.1 200 OK`

```json
{
  "createdAt": "2013-04-07T09:11:51-03:00",
  "event": "order/created",
  "id": 5670,
  "updatedAt": "2013-04-08T11:11:51-03:00",
  "url": "https://myapp.com/order_created_hook"
}
```

### POST /webhooks

Create a new Webhook

#### POST /webhooks

```json
{
  "url": "foobar",
  "event": "invalid_event"
}
```

`HTTP/1.1 400 Bad Request`

```json
{
  "event": [
    "Invalid event specified. Events allowed: app/uninstalled, app/suspended, app/resumed, order/created, order/updated, order/paid, order/cancelled"
  ],
  "url": [
    "invalid url specified"
  ]
}
```

#### POST /webhooks

```json
{
  "event": "order/created",
  "url": "https://myapp.com/product_created_hook"
}
```

`HTTP/1.1 201 Created`

```json
{
  "createdAt": "2013-06-01T15:12:15-03:00",
  "event": "order/created",
  "id": 8901,
  "updatedAt": "2013-06-01T15:12:15-03:00",
  "url": "https://myapp.com/order_created_hook"
}
```

### PUT /webhooks/{id}

Modify an existing Webhook

#### PUT /webhooks/5670

```json
{
  "createdAt": "2013-04-07T09:11:51-03:00",
  "event": "order/created",
  "id": 5670,
  "updatedAt": "2013-04-08T11:11:51-03:00",
  "url": "https://myapp.com/order_created_hook"
}
```

`HTTP/1.1 200 OK`

```json
{
  "createdAt": "2013-04-07T09:11:51-03:00",
  "event": "order/created",
  "id": 5670,
  "updatedAt": "2013-06-01T12:11:14-03:00",
  "url": "https://myapp.com/order_created_hook"
}
```

### DELETE /webhooks/{id}

Remove a Webhook

#### DELETE /webhooks/5670

`HTTP/1.1 200 OK`

```json
{}
```
