## Recurring Documents

A recurring is a special document that periodically renews itself and generates a recurring invoice or expense. You can use to send periodic invoices to your cusstomers or to track recurring business bills. 

### Create a recurring

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/recurring \
  -u YOUR_API_KEY:x \
  -d recurring_document="invoice" \
  -d frequency="monthly" \
  -d start_date="2021-01-15" \
  -d contact[id]=176939 \
  -d contact[first_name]="Veronica Black" \
  -d delivery="send" \
  -d items_attributes[0][description]="Friday stuff" \
  -d items_attributes[0][quantity]=1 \
  -d items_attributes[0][total_amount]=34 \
  -d custom_metadata[a_custom_key]="a custom value"
  ...
```
> The above command returns JSON structured like this:

```json
{
  "id":6420739232,
  "recurring_document":"invoice",
  "start_date":"2015-01-02",
  "end_date":null,
  "frequency":"weekly",
  "recurring_period":"weeks",
  "recurring_frequency":"1",
  "contact":{
    "id":176939,
    "full_name":"Veronica Black"
  },
  "po_number":"",
  "due_days":null,
  "currency":"EUR",
  "items":[
    {
      "id":1589114,
      "description":"Friday stuff",
      "quantity":"1.0",
      "unit_price_cents":"3400",
      "discount_rate":"0.0",
      "tax_1_name":"",
      "tax_1_rate":null,
      "tax_2_name":"",
      "tax_2_rate":null,
      "reference":"",
      "subtotal_cents":"3400",
      "discount_cents":"0",
      "gross_amount_cents":"3400"
    }
  ],
  "subtotal_cents":"3400",
  "discount_cents":"0",
  "taxes":[],
  "total_cents":"3400",
  "payment_details":"",
  "notes":"",
  "state":"active",
  "tag_list":[],
  "custom_metadata":{}
}
```

#### HTTP Request

`POST /recurring`

#### Parameters

Parameter               | Type              | Description
------------------------|-------------------|--------------------------------------------------------------------------------
`recurring_document`    | string            | Can be `invoice` or `expense`. Defaults to `invoice`
`start_date`            | date              | Date of the issue of the first document for this recurring. Defaults to 1 months from today.
`end_date`              | date              | Date of the issue of the last document for this recurring.
`frequency`             | string            | Can be `daily`, `weekly`, `biweekly`, `monthly`, `bimonthly`, `quarterly`, `semiyearly`, `yearly`, `biyearly`. Defaults to `monthly`.
`delivery`              | string            | Can be `send`, `create`, `nothing`. Defaults to `send`.
`currency`              | string            | Three-letter [ISO currency code](https://en.wikipedia.org/wiki/ISO_4217), in uppercase. Defaults to the account's default currency.
`contact`               | object            | The data of the [customer or the provider](#contacts).
`items_attributes`      | array             | Array of [document items](#document-items). **Required**
`po_number`             | string            | The number of the related order. 
`tag_list`              | string            | Multiple tags should be separated by commas
`payment_details`       | string            | Detailed information about the accepted payment methods.
`notes`                 | string            | Extra notes about the invoice.
`custom_metadata`       | hash        | Set of key-value pairs that you can attach to an object. This can be useful for storing additional information about the object in a structured format. You can have up to 20 keys, with key names up to 40 characters long and values up to 500 characters long.








### Retrieve a recurring

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/recurring/RECURRING_ID \
  -u YOUR_API_KEY:x
```
> The above command returns JSON structured like this:

```json
{
  "id":6420739232,
  "recurring_document":"invoice",
  "start_date":"2015-01-02",
  "end_date":null,
  "frequency":"weekly",
  "recurring_period":"weeks",
  "recurring_frequency":"1",
  "contact":{
    "id":176939,
    "full_name":"Veronica Black"
  },
  "po_number":"",
  "due_days":null,
  "currency":"EUR",
  "items":[
    {
      "id":1589114,
      "description":"Friday stuff",
      "quantity":"1.0",
      "unit_price_cents":"3400",
      "discount_rate":"0.0",
      "tax_1_name":"",
      "tax_1_rate":null,
      "tax_2_name":"",
      "tax_2_rate":null,
      "reference":"",
      "subtotal_cents":"3400",
      "discount_cents":"0",
      "gross_amount_cents":"3400"
    }
  ],
  "subtotal_cents":"3400",
  "discount_cents":"0",
  "taxes":[],
  "total_cents":"3400",
  "payment_details":"",
  "notes":"",
  "state":"active",
  "tag_list":[],
  "custom_metadata":{}
}
```

#### HTTP Request

`GET /recurring/RECURRING_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`RECURRING_ID`          | integer   | The ID of the recurring to retrieve. **Required**







### Update a recurring

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/recurring/RECURRING_ID \
  -u YOUR_API_KEY:x \
  -X PUT \
  -d notes="You better pay this time, Tony."
```
> The above command returns JSON structured like this:

```json
{
  "id":6420739232,
  "recurring_document":"invoice",
  "start_date":"2015-01-02",
  "end_date":null,
  "frequency":"weekly",
  "recurring_period":"weeks",
  "recurring_frequency":"1",
  "contact":{
    "id":176939,
    "full_name":"Veronica Black"
  },
  "po_number":"",
  "due_days":null,
  "currency":"EUR",
  "items":[
    {
      "id":1589114,
      "description":"Friday stuff",
      "quantity":"1.0",
      "unit_price_cents":"3400",
      "discount_rate":"0.0",
      "tax_1_name":"",
      "tax_1_rate":null,
      "tax_2_name":"",
      "tax_2_rate":null,
      "reference":"",
      "subtotal_cents":"3400",
      "discount_cents":"0",
      "gross_amount_cents":"3400"
    }
  ],
  "subtotal_cents":"3400",
  "discount_cents":"0",
  "taxes":[],
  "total_cents":"3400",
  "payment_details":"",
  "notes":"",
  "state":"active",
  "tag_list":[],
  "custom_metadata":{}
}
```

#### HTTP Request

`PUT /recurring/RECURRING_ID`

#### Parameters

Parameter               | Type              | Description
------------------------|-------------------|--------------------------------------------------------------------------------------------------------------
`RECURRING_ID`          | integer           | The ID of the recurring to retrieve. **Required**
`end_date`              | date              | Date of the issue of the last document for this recurring.
`delivery`              | string            | Can be `send`, `create`, `nothing`. Defaults to `send`.
`currency`              | string            | Three-letter [ISO currency code](https://en.wikipedia.org/wiki/ISO_4217), in uppercase. Defaults to the account's default currency.
`contact`               | object            | The data of the [customer or the provider](#contacts).
`items_attributes`      | array             | Array of [document items](#document-items). **Required**
`po_number`             | string            | The number of the related order. 
`tag_list`              | string            | Multiple tags should be separated by commas
`payment_details`       | string            | Detailed information about the accepted payment methods.
`notes`                 | string            | Extra notes about the invoice.
`custom_metadata`       | hash        | Set of key-value pairs that you can attach to an object. This can be useful for storing additional information about the object in a structured format. You can have up to 20 keys, with key names up to 40 characters long and values up to 500 characters long.







### Delete a recurring

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/recurring/RECURRING_ID \
  -u YOUR_API_KEY:x \
  -X DELETE
```

#### HTTP Request

`DELETE /recurring/RECURRING_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`RECURRING_ID`          | integer   | The ID of the recurring to delete. **Required**






### List all recurring

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/recurring \
  -u YOUR_API_KEY:x
```
> The above command returns JSON structured like this:

```json
[
  {
    "id":642069232,
    "recurring_document":"expense",
    "start_date":"2015-02-21",
    "end_date":"2015-06-21",
    "frequency":"monthly",
    "recurring_period":"months",
    "recurring_frequency":"1",
    "contact":{
      "id":176938,
      "full_name":"Chuck Testa"
    },
    "po_number":"",
    "due_days":null,
    "currency":"EUR",
    "items":[
      {
        "id":1589108,
        "description":"Something corporate",
        "quantity":"1.0",
        "unit_price_cents":"2300",
        "discount_rate":"0.0",
        "tax_1_name":"",
        "tax_1_rate":null,
        "tax_2_name":"",
        "tax_2_rate":null,
        "reference":"",
        "subtotal_cents":"2300",
        "discount_cents":"0",
        "gross_amount_cents":"2300"
      }
    ],
    "subtotal_cents":"2300",
    "discount_cents":"0",
    "taxes":[],
    "total_cents":"2300",
    "payment_details":"",
    "notes":"",
    "state":"archived",
    "tag_list":[],
    "custom_metadata":{}
  },
  {
    "id":6420739232,
    "recurring_document":"invoice",
    "start_date":"2015-01-02",
    "end_date":null,
    "frequency":"weekly",
    "recurring_period":"weeks",
    "recurring_frequency":"1",
    "contact":{
      "id":176939,
      "full_name":"Veronica Black"
    },
    "po_number":"",
    "due_days":null,
    "currency":"EUR",
    "items":[
      {
        "id":1589114,
        "description":"Friday stuff",
        "quantity":"1.0",
        "unit_price_cents":"3400",
        "discount_rate":"0.0",
        "tax_1_name":"",
        "tax_1_rate":null,
        "tax_2_name":"",
        "tax_2_rate":null,
        "reference":"",
        "subtotal_cents":"3400",
        "discount_cents":"0",
        "gross_amount_cents":"3400"
      }
    ],
    "subtotal_cents":"3400",
    "discount_cents":"0",
    "taxes":[],
    "total_cents":"3400",
    "payment_details":"",
    "notes":"",
    "state":"active",
    "tag_list":[],
    "custom_metadata":{}
  }
]
```

#### HTTP Request

`GET /recurring`

#### Parameters

Parameter      | Type      | Description
---------------|-----------|----------------------------------------------------------------------------
`q`            | string    | A case-sensitive filter on the list based on the recurring's contact or PO number. 
`date`         | string    | A case-sensitive filter on the list based on the invoice's issue date. Pass the date range in the url like `?date=DATE1,DATE2`. Date range allows these formats: `2019-01-01,2019-12-31` or `2019/01/01,2019/12/31`.
`state`        | string    | A case-sensitive filter on the list based on the invoice's state. Can be `active` and `archived`. 
`contact`      | integer    | A case-sensitive filter on the list based on the contact's ID.
