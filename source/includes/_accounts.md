# CONNECT 

## Accounts (beta)

When using Quaderno Connect, you need to create an account (known as a connected account) for each user that needs to calculate taxes on your platform. These accounts are generally created when a user signs up for your platform.

<aside class="notice">
Quaderno Connect is only available with the Business plan or higher.
</aside>

### Create an account

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/accounts \
  -u YOUR_API_KEY:x \
  -d business_name="Big Bang Inc." \
  -d type="custom" \
  -d country="US" \
  -d postal_code="10001" \
  -d email="s.cooperphd@yahoo.com"
```

> The above command returns JSON structured like this:

```json
{
    "id": 456987213,
    "type":"custom",
    "business_name":"Sheldon",
    "country":"US",
    "currency":"USD",
    "email":"s.cooperphd@yahoo.com",
    "state":"active",
    "created_at":"1611167152"
}
```

#### HTTP Request

`POST /accounts`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`business_name`         | string    | The account's business name. **Required**
`email`                 | string    | The account's email address. **Required**
`country`               | string    | The account's country. 2-letter country code. **Required**
`postal_code`           | string    | The account's postal code. Recommended whether the account is based in the US or Canada to determine the state/province.
`currency`              | string    | The account's accounting currency. Defaults to the country's official currency.
`local_tax_id`          | string    | The account's tax ID. If set, the account will be registered for tax collection in the `country`'s jurisdiction.

### Retrieve an account

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/accounts/ACCOUNT_ID \
  -u YOUR_API_KEY:x \
```

> The above command returns JSON structured like this:

```json
{
    "id": 456987213,
    "type":"custom",
    "business_name":"Sheldon",
    "country":"US",
    "currency":"USD",
    "email":"s.cooperphd@yahoo.com",
    "state":"active",
    "created_at":"1611167152"
}
```

#### HTTP Request

`GET /accounts/ACCOUNT_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`ACCOUNT_ID`            | integer   | The ID of the account to retrieve. **Required**

### Update an account

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/accounts/ACCOUNT_ID \
  -u YOUR_API_KEY:x \
  -X PUT \
  -d business_name="Another Name Inc."
```

> The above command returns JSON structured like this:

```json
{
    "id": 456987213,
    "type":"custom",
    "business_name":"Sheldon",
    "country":"US",
    "currency":"USD",
    "email":"s.cooperphd@yahoo.com",
    "state":"active",
    "created_at":"1611167152"
}
```

#### HTTP Request

`PUT /accounts/ACCOUNT_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`ACCOUNT_ID`            | integer   | The ID of the account to retrieve. **Required**
`business_name`         | string    | The accounts's business name.
`email`                 | string    | The account's email address.

### List all accounts

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/accounts \
  -u YOUR_API_KEY:x
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 456987213,
    "type":"custom",
    "business_name":"Sheldon",
    "country":"US",
    "currency":"USD",
    "email":"s.cooperphd@yahoo.com",
    "state":"active",
    "created_at":"1611167152"
  },
  {
    "id": 456982365,
    "type":"custom",
    "business_name":"Apple Inc.",
    "country":"US",
    "currency":"USD",
    "email":"info@apple.com",
    "state":"active",
    "created_at":"1611167152"
  }
]
```

#### HTTP Request

`GET /accounts`

