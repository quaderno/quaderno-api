---
title: Quaderno API Reference

language_tabs:
  - curl
  - ruby
  - php
  - swift

toc_footers:
  - <a href='http://quaderno.io'>Sign Up for an Account</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

> Libraries for the Quaderno API are [available in several languages](http://support.quaderno.io/article/100-our-api-libraries).

Welcome to the Quaderno API! You can use our API to access all our features, designed around the idea of making tax management and invoicing easy for small businesses.

The Quaderno API is based on the [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) architectural style, and comprised of resources with predictable, human-readable and resource-oriented URLs. We make use of standard HTTP features (HTTP Authentication, Verbs, and Response Codes), understandable by any off-the-shelf HTTP client. We return [JSON](http://www.json.org/) for everything (including errors!), and expect to receive JSON for all `POST` and `PUT` requests.

We have language bindings for the Shell, [Ruby](https://github.com/quaderno/quaderno-ruby), [PHP](https://github.com/quaderno/quaderno-php) and [Swift](https://github.com/quaderno/quaderno-swift)! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top-right.

If you have any suggestions, tips, or questions that you feel aren't answered here or in our [Knowledge Base](http://support.quaderno.io), please [get in touch](mailto:hello@quaderno.io) and let us know!

# Endpoint URL Structure

> API Endpoint

```
https://ACCOUNT-NAME.quadernoapp.com/api/v1/
```

All URLs start with the root `https://ACCOUNT-NAME.quadernoapp.com/api/v1/`.

- `ACCOUNT-NAME` is different for each user, and is used as the identifier for which user to affect with an API call. You can see this in the URL bar when logging into your Quaderno account.
- Note that the API version is also included in the root of every call. This ensures that updates to the API will not break older code, but when you are ready to make the switch (and when we have a new version) you can do so by updating this root.

The resource in question will follow, like so:

`https://ACCOUNT-NAME.quadernoapp.com/api/v1/RESOURCE.json`

# Making a request

> A basic GET for a user's contacts looks like this:

```
# :x stops cURL from prompting for a password
curl -u YOUR_API_KEY:x
     -X GET 'https://ACCOUNT-NAME.quadernoapp.com/api/v1/contacts.json'
```

> A POST with a new contact looks like this:

```
curl -u YOUR_API_KEY:x
     -H 'Content-Type: application/json'
     -X POST
     -d {"first_name":"Tony", "kind":"person", "contact_name":"Stark"}
     'https://ACCOUNT-NAME.quadernoapp.com/api/v1/contacts.json'
```

As mentioned, we use standard HTTP verbs for the API requests: `GET`,`POST` and `PUT`.

Every request involves JSON (even in error cases), and we only support JSON for serialisation of data.

Our format is to have:

- No root element
- `snake_case` to describe attribute keys

When sending JSON (in `PUT` or `POST` requests), you must specify `Content-Type: application/json;` as the header of the HTTP request.

<aside class="notice">
All API URLs end in `.json` to indicate that they accept and return JSON.
</aside>

# Authentication

> To authorize, use this code:

```ruby
Quaderno::Base.configure do |config|
    config.auth_token = 'YOUR_API_KEY'
    config.url = 'YOUR_API_URL'
end
```

```php
require_once 'quaderno_load.php';

QuadernoBase::init('YOUR_API_KEY',
                   'YOUR_API_URL');
```

```swift
let client = Quaderno.Client(baseURL: "YOUR_API_URL",
							 authenticationToken: "YOUR_API_KEY")
```

```curl
# With curl, you can pass the API key as a header with each request
curl -u YOUR_API_KEY:x
     -X GET
     'https://ACCOUNT-NAME.quadernoapp.com/api/v1/invoices.json'
```

> Make sure to replace `YOUR_API_KEY` with your API key.

Quaderno uses an API key to authorise all requests, allowing access to the API in combination with the account name in the endpoint URL. For more info on finding your API key, check [here](http://support.quaderno.io/article/101-how-do-i-get-my-api-key).

Quaderno expects the API key to be included in all API requests to the server in a header that looks like the following:

`YOUR_API_KEY:x`

<aside class="notice">
You must replace `YOUR_API_KEY` with your personal API key.
No password is required.
</aside>

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```curl
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```curl
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve
