# CORE RESOURCES

## Contacts

A contact is any customer or vendor who appears on your invoices, credit notes, and expenses.

### Create a contact

> `POST /contacts.json`

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/contacts.json \
  -u YOUR_API_KEY:x \
  -d first_name="Sheldon" \
  -d last_name="Cooper" \
  …
```
> The above command returns JSON structured like this:

```json
{
    "id":456987213,
    "kind":"person",
    "first_name":"Sheldon",
    "last_name":"Cooper",
    "full_name":"Sheldon Cooper",
    "street_line_1":"2311 N. Los Robles Avenue",
    "street_line_2":"",
    "postal_code":"91104",
    "city":"Pasadena",
    "region":"CA",
    "country":"US",
    "phone_1":"",
    "email":"s.cooperphd@yahoo.com",
    "web":"",
    "discount":null,
    "tax_id":"",
    "language":"EN",
    "notes":"",
    "permalink":"https://ACCOUNT_NAME.quadernoapp.com/billing/th3p3rm4l1nk",
}
```
#### HTTP Request

`POST /contacts`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`kind`                  | string    | The type of contact. Values are `company` or `person`. Defaults to `company`.
`first_name`            | string    | The contact's first name / business name. **Required**
`last_name`             | string    | The contact's last name. Empty when the contact is a `company`.
`tax_id`                | string    | Any tax identification number. Quaderno can validate tax IDs from the EU, United Kingdom, Switzerland, Québec (Canada), Australia, and New Zealand.
`contact_person`        | string    | If the contact is a `company`, this is its contact person.
`street_line_1`         | string    | Address line 1 (Street address/PO Box).
`street_line_2`         | string    | Address line 2 (Apartment/Suite/Unit/Building).
`city`                  | string    | City/District/Suburb/Town/Village.
`postal_code`           | string    | ZIP or postal code.
`region`                | string    | Region, province or state.
`country`               | string    | 2-letter country code.
`phone_1`               | string    | The contact's phone number.
`email`                 | string    | The contact's email address. Multiple emails should be separated by commas. Maximum number of addresses allowed is 3.
`web`                   | string    | The contact's website. Validates format
`language`              | string    | The contact's preferred language. Should be included in the account's translations list
`notes`                 | string    | Internal notes about the contact.




### Retrieve a contact

> `GET /contacts/CONTACT_ID.json`

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/contacts/CONTACT_ID \
  -u YOUR_API_KEY:x
```

> The above command returns JSON structured like this:

```json
{
    "id":456987213,
    "kind":"person",
    "first_name":"Sheldon",
    "last_name":"Cooper",
    "full_name":"Sheldon Cooper",
    "street_line_1":"2311 N. Los Robles Avenue",
    "street_line_2":"",
    "postal_code":"91104",
    "city":"Pasadena",
    "region":"CA",
    "country":"US",
    "phone_1":"",
    "email":"s.cooperphd@yahoo.com",
    "web":"",
    "discount":null,
    "tax_id":"",
    "language":"EN",
    "notes":"",
    "permalink":"https://ACCOUNT_NAME.quadernoapp.com/billing/th3p3rm4l1nk",
}
```

#### HTTP Request

`GET /contacts/CONTACT_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`CONTACT_ID`            | integer   | The ID of the contact to retrieve. **Required**





### Retrieve a contact by payment processor

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/PAYMENT_PROCESSOR/customers/CUSTOMER_ID \
  -u YOUR_API_KEY:x
```

> The above command returns JSON structured like this:

```json
{
    "id":456987213,
    "kind":"person",
    "first_name":"Sheldon",
    "last_name":"Cooper",
    "full_name":"Sheldon Cooper",
    "street_line_1":"2311 N. Los Robles Avenue",
    "street_line_2":"",
    "postal_code":"91104",
    "city":"Pasadena",
    "region":"CA",
    "country":"US",
    "phone_1":"",
    "email":"s.cooperphd@yahoo.com",
    "web":"",
    "discount":null,
    "tax_id":"",
    "language":"EN",
    "notes":"",
    "permalink":"https://ACCOUNT_NAME.quadernoapp.com/billing/th3p3rm4l1nk",
}
```

#### HTTP Request

`GET /PAYMENT_PROCESSOR/customers/CUSTOMER_ID.json`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`PAYMENT_PROCESSOR`     | string    | The name of the payment processor. Can be `stripe`, `paypal`, `gocardless` and `braintree`. **Required**
`CUSTOMER_ID`           | string    | The ID of the contact in the payment processor. **Required**





### Update a contact

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/contacts/CONTACT_ID \
  -u YOUR_API_KEY:x \
  -X PUT \
  -d first_name="Anthony"
```

> The above command returns JSON structured like this:

```json
{
    "id":456987213,
    "kind":"person",
    "first_name":"Anthony",
    "last_name":"Cooper",
    "full_name":"Sheldon Cooper",
    "street_line_1":"2311 N. Los Robles Avenue",
    "street_line_2":"",
    "postal_code":"91104",
    "city":"Pasadena",
    "region":"CA",
    "country":"US",
    "phone_1":"",
    "email":"s.cooperphd@yahoo.com",
    "web":"",
    "discount":null,
    "tax_id":"",
    "language":"EN",
    "notes":"",
    "permalink":"https://ACCOUNT_NAME.quadernoapp.com/billing/th3p3rm4l1nk",
}
```

#### HTTP Request

`PUT /contacts/CONTACT_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`CONTACT_ID`            | integer   | The ID of the contact to update. **Required**
`kind`                  | string    | The type of contact. Values are `company` or `person`. Defaults to `company`.
`first_name`            | string    | The contact's first name. **Required**
`last_name`             | string    | The contact's last name. Empty when the contact is a `company`.
`tax_id`                | string    | Any tax identification number. Quaderno can validate tax IDs from the EU, United Kingdom, Switzerland, Québec (Canada), Australia, and New Zealand.
`contact_person`        | string    | If the contact is a `company`, this is its contact person.
`street_line_1`         | string    | Address line 1 (Street address/PO Box).
`street_line_2`         | string    | Address line 2 (Apartment/Suite/Unit/Building).
`city`                  | string    | City/District/Suburb/Town/Village.
`postal_code`           | string    | ZIP or postal code.
`region`                | string    | Region, province or state.
`country`               | string    | 2-letter country code.
`phone_1`               | string    | The contact's phone number.
`email`                 | string    | The contact's email address. Multiple emails should be separated by commas. Maximum number of addresses allowed is 3.
`web`                   | string    | The contact's website. Validates format
`language`              | string    | The contact's preferred language. Should be included in the translations list
`notes`                 | string    | Internal notes about the contact.






### Delete a contact

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/contacts/CONTACT_ID \
  -u YOUR_API_KEY:x \
  -X DELETE
```
#### HTTP Request

`DELETE /contacts/CONTACT_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`CONTACT_ID`            | integer   | The ID of the contact to delete. **Required**






### List all contacts

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/contacts \
  -u YOUR_API_KEY:x 
```

> The above command returns JSON structured like this:

```json
[
  {
    "id":456987213,
    "kind":"person",
    "first_name":"Sheldon",
    "last_name":"Cooper",
    "full_name":"Sheldon Cooper",
    "street_line_1":"2311 N. Los Robles Avenue",
    "street_line_2":"",
    "postal_code":"91104",
    "city":"Pasadena",
    "region":"CA",
    "country":"US",
    "phone_1":"",
    "email":"s.cooperphd@yahoo.com",
    "web":"",
    "discount":null,
    "tax_id":"",
    "language":"EN",
    "notes":"",
    "permalink":"https://ACCOUNT_NAME.quadernoapp.com/billing/th3p3rm4l1nk",
  },
  {
    "id":456982365,
    "kind":"company",
    "full_name":"Apple Inc.",
    "street_line_1":"1 Infinite Loop",
    "street_line_2":"",
    "postal_code":"95014",
    "city":"Cupertino",
    "region":"CA",
    "country":"US",
    "phone_1":"",
    "email":"info@apple.com",
    "web":"http://apple.com",
    "discount":null,
    "tax_id":"",
    "language":"EN",
    "notes":"",
    "permalink":"https://ACCOUNT_NAME.quadernoapp.com/billing/4n0th3rp3rm4l1nk",
  }
]
```

#### HTTP Request

`GET /contacts`

#### Parameters

Parameter      | Type      | Description
---------------|-----------|----------------------------------------------------------------------------
`q`            | string    | A case-sensitive filter on the list based on the contact's full name, email or tax ID.
