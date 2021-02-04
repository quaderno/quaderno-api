# WEBHOOKS 

## Webhooks Endpoints

Webhooks refers to a combination of elements that collectively create a notification and reaction system within a larger integration.

Metaphorically, webhooks are like a phone number that Quaderno calls to notify you of activity in your Quaderno account. The activity could be the creation of a new customer or the payment of an invoice. The webhook endpoint is the person answering that call who takes actions based upon the specific information it receives.

Non-metaphorically, the webhook endpoint is just more code on your server, which could be written in Ruby, PHP, Node.js, or whatever. The webhook endpoint has an associated URL (e.g., https://example.com/webhooks). The Quaderno notifications are [`Event`](\#events) objects. This `Event` object contains all the relevant information about what just happened, including the type of event and the data associated with that event. The webhook endpoint uses the event details to take any required actions, such as indicating that an order should be fulfilled.

### Create a webhook

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/webhooks \
  -u YOUR_API_KEY:x \
  -d url="http://anotherapp.com/notifications" \
  -d event_types[]="invoice.created" \
  -d event_types[]="invoice.deleted" \
  -d event_types[]="contact.created" 
```
> The above command returns JSON structured like this:

```json
{
    "id":3,
    "url":"http://anotherapp.com/notifications",
    "auth_key":"HXQgAgblQxAMaNppMrXoSW",
    "events:_types":["invoice.created","invoice.deleted","contact.created"],
    "last_sent_at":null,
    "last_error":null,
    "events_sent":null,
    "created_at":"2013-07-13T14:12:01Z",
    "updated_at":"2013-07-17T14:09:59Z"
}
```


#### HTTP Request

`POST /webhooks`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`url`                   | string    | The destination URL of the webhook request. **Required**
`events_types`          | array     | An array of strings indicating which events you wish to subscribe to. **Required**


Webhook URLs should be set up to accept `HEAD` and `POST` requests. When you provide the URL where you want Quaderno to `POST` the data for events, we'll do a quick check that the URL exists by using a `HEAD` request (not `POST`).

If the URL doesn't exist or returns something other than a `200` HTTP response to the `HEAD` request, Quaderno won't be able to verify that the URL exists and is valid.

This will return `201 Created` with the current JSON representation of the webhook if the creation was a success.





### Retrieve a webhook

> GET /webhooks/WEBHOOK_ID.json

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/webhooks/WEBHOOK_ID \
  -u YOUR_API_KEY:x
```
> The above command returns JSON structured like this:

```json
{
    "id":3,
    "url":"http://anotherapp.com/notifications",
    "auth_key":"HXQgAgblQxAMaNppMrXoSW",
    "events:_types":["invoice.created","contact.deleted"],
    "last_sent_at":null,
    "last_error":null,
    "events_sent":null,
    "created_at":"2013-07-13T14:12:01Z",
    "updated_at":"2013-07-17T14:09:59Z"
}
```
#### HTTP Request

`GET /webhooks/WEBHOOK_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`WEBHOOK_ID`            | integer   | The ID of the webhook to retrieve. **Required**





### Update a webhook

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/webhooks/WEBHOOK_ID \
  -u YOUR_API_KEY:x \
  -X PUT \
  -d url="http://anotherapp.com/notifications" \
  -d event_types[]="contact.updated" \
  -d event_types[]="contact.created" 
```
> The above command returns JSON structured like this:

```json
{
    "id":3,
    "url":"http://anotherapp.com/notifications",
    "auth_key":"HXQgAgblQxAMaNppMrXoSW",
    "events:_types":["contact.updated","contact.created"],
    "last_sent_at":null,
    "last_error":null,
    "events_sent":null,
    "created_at":"2013-07-13T14:12:01Z",
    "updated_at":"2013-07-17T14:09:59Z"
}
```

#### HTTP Request

`PUT /webhooks/WEBHOOK_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`WEBHOOK_ID`            | integer   | The ID of the webhook to retrieve. **Required**
`url`                   | string    | The destination URL of the webhook request. **Required**
`events_types`          | array     | An array of strings indicating which events you wish to subscribe to. **Required**




### Delete a webhook


```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/webhooks/WEBHOOK_ID \
  -u YOUR_API_KEY:x \
  -X DELETE
```
#### HTTP Request

`DELETE /webhooks/WEBHOOK_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`WEBHOOK_ID`            | integer   | The ID of the webhook to delete. **Required**






### List all webhooks

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/webhooks \
  -u YOUR_API_KEY:x
```
> The above command returns JSON structured like this:

```json
[
  {
    "id":2,
    "url":"https://myawesomeapp.com/notifications",
    "auth_key":"zXQgArTtQxAMaYppMrDoUQ",
    "events":["created","updated"],
    "last_sent_at":"2013-05-18T11:11:11Z",
    "last_error":null,
    "events_sent":null,
    "created_at":"2013-05-17T14:08:05Z",
    "updated_at":"2013-05-17T14:08:05Z"
  }
  {
    "id":3,
    "url":"http://anotherapp.com/notifications",
    "auth_key":"HXQgAgblQxAMaNppMrXoSW",
    "events:_types":["invoice.created","contact.deleted"],
    "last_sent_at":null,
    "last_error":null,
    "events_sent":null,
    "created_at":"2013-07-13T14:12:01Z",
    "updated_at":"2013-07-17T14:09:59Z"
  }
]
```

#### HTTP Request

`GET /webhooks`
