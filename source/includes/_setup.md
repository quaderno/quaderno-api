# Getting Started
## Endpoint URL Structure

> API Endpoint

```
https://ACCOUNT_NAME.quadernoapp.com/api/v1/
```

All URLs start with the root `https://ACCOUNT_NAME.quadernoapp.com/api/v1/`.

- `ACCOUNT_NAME` is different for each user, and is used as the identifier for which user to affect with an API call. You can see this in the URL bar when logging into your Quaderno account.
- Note that the API version is also included in the root of every call. This ensures that updates to the API will not break older code, but when you are ready to make the switch (and when we have a new version) you can do so by updating this root.

The resource in question will follow, like so:

`https://ACCOUNT_NAME.quadernoapp.com/api/v1/RESOURCE.json`

## Authentication

> To authorize, use this code:

```ruby
Quaderno::Base.configure do |config|
    config.auth_token = 'YOUR_API_KEY'
    config.url = 'YOUR_API_URL'
end
```

```php?start_inline=1
require_once 'quaderno_load.php';

QuadernoBase::init('YOUR_API_KEY',
                   'YOUR_API_URL');
```

```swift?start_inline=1
let client = Quaderno.Client(baseURL: "YOUR_API_URL",
							 authenticationToken: "YOUR_API_KEY")
```

```shell
# With curl, you can pass the API key as a header with each request
curl -u YOUR_API_KEY:x \
     -X GET \
     'https://ACCOUNT_NAME.quadernoapp.com/api/v1/invoices.json'
```

> You can `ping` to check if the service is up, your credentials are correct, or to know your remaining requests without doing an actual request:

```ruby
Quaderno::Base.ping #=> Boolean
```

```shell
curl -u YOUR_API_KEY:x \
     -X GET \
     'https://quadernoapp.com/api/v1/ping.json'
```

```php?start_inline=1
QuadernoBase::ping();   // Returns true (success) or false (error)
```

```swift?start_inline=1
let client = Quaderno.Client(/* ... */)
client.ping { success in
  // success will be true if the service is available.
}
```

Quaderno uses an API key to authorise all requests, allowing access to the API in combination with the account name in the endpoint URL. For more info on finding your API key, check [here](http://support.quaderno.io/article/101-how-do-i-get-my-api-key).

Quaderno expects the API key to be included via HTTP Basic Auth in all API requests to the server, in a header that looks like the following:

`-u YOUR_API_KEY:x`

<aside class="notice">
You must replace YOUR_API_KEY with your personal API key.
No password is required.
</aside>

<aside class="warning">
Remember that anyone who has your Private Key can see and change everything that you have access to. Make sure to keep it, and your password, safe.
</aside>

## Authorization

```shell
curl -u YOUR_API_KEY:x \
     -X GET \
     'https://quadernoapp.com/api/v1/authorization.json'
```

```ruby
Quaderno::Base.authorization 'my_authenticate_token', environment
# environment is an optional argument. By passing :sandbox,
# you will retrieve your credentials for the sandbox
# environment and not for production.
```

```swift?start_inline=1
let client = Quaderno.Client(/* ... */)
client.account { credentials in
  // credentials will contain the account credentials.
}
```

```php?start_inline=1
// Erk, TODO!
```

> Returns:

```json
{
    "id": "999",
    "name": "Sheldon Cooper",
    "email": "s.cooperphd@yahoo.com",
    "href": "http://nippur-999.quadernoapp.com/api/v1/"
}
```

If you don't know the `ACCOUNT_NAME` for your target account, you can get it with the `authorization` API call.

This returns the following as a JSON payload:

**Attribute** | **Description**
--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
`id`          | An identity, which is *not* used for determining who this user is within Quaderno. The `id` field should therefore *not* be used for submitting data within Quaderno's API.
`name`        | The user's full name.
`email`       | The user's email address.
`href`        | The custom API endpoint URL for the user, providing the sought-after `ACCOUNT_NAME` between `http://` and `.quadernoapp...`.

<aside class="notice">
Note that in all cases, if the user does not have permission to do something, you'll get a 401 Unauthorized error.
</aside>

## Making a request

> A basic GET for a user's contacts looks like this:

```shell
# :x stops cURL from prompting for a password
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/v1/contacts.json'
```

```php?start_inline=1
// Returns an array of QuadernoContact
$contacts = QuadernoContact::find();  
```

```ruby
Quaderno::Contact.all() #=> Array
```

```swift?start_inline=1
let client = Quaderno.Client(/* ... */)

let readContact = Contact.list(pageNum)
client.request(readContact) { response in
  // response will contain the result of the request.
}
```

> A POST with a new contact looks like this:

```shell
curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X POST \
     -d {"first_name":"Tony", "kind":"person", "contact_name":"Stark"}
     'https://ACCOUNT_NAME.quadernoapp.com/api/v1/contacts.json'
```

```php?start_inline=1
$contact = new QuadernoContact(array(
                                 'first_name' => 'Tony',
                                 'kind' => 'person',
                                 'contact_name' => 'Stark'));

$contact->save(); // Returns true (success) or false (error)
```

```ruby
# Using new hash syntax
params = {
    first_name: 'Tony',
    kind: 'person',
    contact_name: 'Stark'
}
Quaderno::Contact.create(params) #=> Quaderno::Contact
```

```swift?start_inline=1
TODO!
```

As mentioned, we use standard HTTP verbs for the API requests: `GET`,`POST`, `PUT` and `DELETE`.

Every request involves JSON (even in error cases), and we only support JSON for serialization of data.

Our format is to have:

- No root element
- `snake_case` to describe attribute keys

When sending JSON (in `PUT` or `POST` requests), you must specify `Content-Type: application/json;` as the header of the HTTP request.

<aside class="notice">
All API URLs end in .json to indicate that they accept and return JSON.
</aside>

## JSON & cURL

### POSTing large amounts of JSON

> `POST /items.json` (with a large JSON body to send)

```curl
# body.json
{
  "code":"BluRay 0003",
  "name":"Titanic IV: The revenge",
  "unit_cost":"15.0",
  "tax_1_name":"AWESOME_TAX",
  "tax_1_rate":"7.00",
  "tax_2_name":"ANOTHER_AWESOME_TAX",
  "tax_2_rate":"10.00",
  "stock":"1000"
}

curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X POST \
     --data-binary @body.json \
     'https://ACCOUNT_NAME.quadernoapp.com/api/v1/items.json'
```

Multiline cURL with JSON is a little bit tricky. In our experience the cleanest, easiest way to make it work correctly is actually to save the desired JSON payload to a file (we use `body.json` as the filename in our examples) and pass it to the cURL command in the `--data-binary` flag, effectively passing the file in by stdin.

Your mileage may vary, but this is the way that we recommend.

### Pretty printing JSON responses

A fun bit of useful cURLing-with-JSON-related fun is to append one of the following commands to your request:

- `| json_pp`
- `| python -mjson.tool > out.json` (on systems with Python installed - this will write to a file)
- `| jq .`

This will cause the JSON to "pretty print", meaning that it will come out nicely formatted like the examples in these docs instead of as one big jumbled mess.

Different commands work better under different circumstances, so check which works best on your system. For us, on OS X 10.11, `| json_pp` is the winner for quick in-terminal checks, and `| python -mjson.tool > out.json` for writing results to files for later perusal.

These will also be useful with other types of HTTP requests, but most useful when getting large chunks of data, as in `list`-type calls.

## Rate Limiting

> To make it easier to determine if your application is being rate-limited, or is approaching that level, we have the following HTTP headers on our successful responses:

```
X-RateLimit-Reset
X-RateLimit-Remaining
```

```ruby
Quaderno::Base.rate_limit_info #=>  {:reset=>4, :remaining=>0}
```

```swift?start_inline=1
// You can check the entitlements for using the service (e.g. the rate
// limit) by inspecting the `entitlements` property of `Client`.
// See the quaderno/quaderno-swift docs for more.
```

There is a limit of **100 API calls per 15 seconds**.

We reserve the right to tune the limitations, but we promise to keep them high enough to allow a well-behaving interactive app to do it's job.

If you exceed the limit you will receive a `HTTP 503 (Service Unavailable)`.

## Pagination

> These HTTP headers inform you about the page context:

```
X-Pages-CurrentPage
X-Pages-TotalPages
```

> A call with the `page` parameter set:

```shell
curl -u YOUR_API_KEY:x \
     -X GET \
     'https://ACCOUNT_NAME.quadernoapp.com/api/v1/contacts.json?page=2'
```

```php?start_inline=1
// Returns an array of QuadernoContact
$contacts = QuadernoContact::find(array('page' => 2));  
```

```ruby
Quaderno::Contact.all(page: 1) #=> Array
```

```swift?start_inline=1
// TODO!
```

Bear in mind that Quaderno paginates `GET` index results, providing **25 results per page**.

You can change the page by passing the `page` parameter, defaulting to `1`.

## Versioning

When we make breaking changes to our API, we release a new version number which you can change in your calls.

<aside class="notice">
The current version is <strong>v1</strong>.
</aside>

## Testing

We recommend testing against our Sandbox environment. You can sign up for a sandbox account (test-ready) [here](http://sandbox-quadernoapp.com).
