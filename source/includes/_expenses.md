## Expenses

Expenses are all the expenses that you receive from your providers.

### Create an expense

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/expenses \
  -u YOUR_API_KEY:x \
  -d contact[id]=999999 \
  -d contact[first_name]="Teenage Mutant Ninja Turtles" \
  -d currency="USD" \
  -d items_attributes[0][description]="Rocket launcher" \
  -d items_attributes[0][quantity]=1 \
  -d items_attributes[0][total_amount]=6000 \
  -d custom_metadata[a_custom_key]="a custom value"
  ...
```

> The above command returns JSON structured like this:

```json
{
  "id":"5076a6b92f412e0e2e00006c",
  "issue_date":"2012-10-11",
  "created_at":"1325376000",
  "contact":{
    "id":999999,
    "full_name":"ACME"
  },
  "country":"US",
  "street_line_1":"Verizon Center",
  "street_line_2":"",
  "city":"Washington DC",
  "region":"Washington DC",
  "postal_code":"",
  "po_number":"",
  "currency":"USD",
  "items":[
    {
      "id":"48151623423",
      "description":"Rockets",
      "quantity":"50.0",
      "unit_price_cents":"12500",
      "discount_rate":"0.0",
      "tax_1_name":"",
      "tax_1_rate":"",
      "tax_2_name":"",
      "tax_2_rate":"",
      "subtotal_cents":"625000",
      "discount_cents":"0",
      "gross_amount_cents":"625000"
    }
  ],
  "subtotal_cents":"625000",
  "discount_cents":"0",
  "taxes":[],
  "total_cents":"625000",
  "payments":[],
  "tag_list":["rockets", "acme"],
  "notes":"",
  "state":"outstanding"
}
```

#### HTTP Request

`POST /expenses`

#### Parameters

Parameter               | Type              | Description
------------------------|-------------------|--------------------------------------------------------------------------------
`issue_date`            | date              | The date of the expense's issue – not necessarily the date the products or services were provided.
`currency`              | string            | Three-letter [ISO currency code](https://en.wikipedia.org/wiki/ISO_4217), in uppercase. Defaults to the account's default currency.
`contact`               | object            | The data of the [provider](#contacts).
`items_attributes`      | array             | Array of [document items](#document-items). **Required**
`payment_method`        | string            | Use this parameter to mark this document as paid in a single request. Can be `credit_card`, `cash`, `wire_transfer`, `direct_debit`, `check`, `iou`, `paypal` or `other`
`payment_processor`     | string            | The name of the payment processor used to take the payment.
`payment_processor_id`  | string            | The ID of the transaction in the `payment_processor`.
`po_number`             | string            | The number of the related order. 
`tag_list`              | string            | Multiple tags should be separated by commas
`payment_details`       | string            | Detailed information about the accepted payment methods.
`notes`                 | string            | Extra notes about the expense.
`attachment`            | object            | The data of the [file to be attached](#attachments).
`custom_metadata`       | hash              | Set of key-value pairs that you can attach to an object. This can be useful for storing additional information about the object in a structured format. You can have up to 20 keys, with key names up to 40 characters long and values up to 500 characters long.






### Retrieve an expense

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/expenses/EXPENSE_ID \
  -u YOUR_API_KEY:x
```
> The above command returns JSON structured like this:

```json
{
  "id":"5076a6b92f412e0e2e00006c",
  "issue_date":"2012-10-11",
  "created_at":"1325376000",
  "contact":{
    "id":999999,
    "full_name":"ACME"
  },
  "country":"US",
  "street_line_1":"Verizon Center",
  "street_line_2":"",
  "city":"Washington DC",
  "region":"Washington DC",
  "postal_code":"",
  "po_number":"",
  "currency":"USD",
  "items":[
    {
      "id":"48151623423",
      "description":"Rockets",
      "quantity":"50.0",
      "unit_price_cents":"12500",
      "discount_rate":"0.0",
      "tax_1_name":"",
      "tax_1_rate":"",
      "tax_2_name":"",
      "tax_2_rate":"",
      "subtotal_cents":"625000",
      "discount_cents":"0",
      "gross_amount_cents":"625000"
    }
  ],
  "subtotal_cents":"625000",
  "discount_cents":"0",
  "taxes":[],
  "total_cents":"625000",
  "payments":[],
  "tag_list":["rockets", "acme"],
  "notes":"",
  "state":"outstanding"
}
```

#### HTTP Request

`GET /expenses/EXPENSE_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`EXPENSE_ID`            | integer   | The ID of the expense to retrieve. **Required**









### Update an expense

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/expenses/EXPENSE_ID \
  -u YOUR_API_KEY:x \
  -X PUT \
  -d payment_details="Money in the bank"
```
> The above command returns JSON structured like this:

```json
{
  "id":"5076a6b92f412e0e2e00006c",
  "issue_date":"2012-10-11",
  "created_at":"1325376000",
  "contact":{
    "id":999999,
    "full_name":"ACME"
  },
  "country":"US",
  "street_line_1":"Verizon Center",
  "street_line_2":"",
  "city":"Washington DC",
  "region":"Washington DC",
  "postal_code":"",
  "po_number":"",
  "currency":"USD",
  "items":[
    {
      "id":"48151623423",
      "description":"Rockets",
      "quantity":"50.0",
      "unit_price_cents":"12500",
      "discount_rate":"0.0",
      "tax_1_name":"",
      "tax_1_rate":"",
      "tax_2_name":"",
      "tax_2_rate":"",
      "subtotal_cents":"625000",
      "discount_cents":"0",
      "gross_amount_cents":"625000"
    }
  ],
  "subtotal_cents":"625000",
  "discount_cents":"0",
  "taxes":[],
  "total_cents":"625000",
  "payments":[],
  "tag_list":["rockets", "acme"],
  "notes":"",
  "state":"outstanding"
}
```

#### HTTP Request

`PUT /expenses/EXPENSE_ID`

#### Parameters

Parameter               | Type              | Description
------------------------|-------------------|--------------------------------------------------------------------------------------------------------------
`EXPENSE_ID`            | integer           | The ID of the expense to update. **Required**
`issue_date`            | date              | The date of the expense's issue - not necessarily the date the products or services were provided.
`contact`               | object            | The data of the [provider](#contacts).
`items_attributes`      | array             | Array of expense items. **Required**
`payment_processor`     | string            | The name of the payment processor used to take the payment.
`payment_processor_id`  | string            | The ID of the transaction in the `payment_processor`.
`po_number`             | string            | The number of the related order. 
`tag_list`              | string            | Multiple tags should be separated by commas
`payment_details`       | string            | Detailed information about the accepted payment methods.
`notes`                 | string            | Extra notes about the expense.
`attachment`            | object            | The data of the [file to be attached](#attachments).
`custom_metadata`       | hash        | Set of key-value pairs that you can attach to an object. This can be useful for storing additional information about the object in a structured format. You can have up to 20 keys, with key names up to 40 characters long and values up to 500 characters long.









### Delete an expense

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/expenses/EXPENSE_ID \
  -u YOUR_API_KEY:x \
  -X DELETE
```

#### HTTP Request

`DELETE /expenses/EXPENSE_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`EXPENSE_ID`            | integer   | The ID of the expense to delete. **Required**









### List all expenses

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/expenses \
  -u YOUR_API_KEY:x
```
> The above command returns JSON structured like this:

```json
[
  {
    "id":"5076a6b92f412e0e2e00006c",
    "issue_date":"2012-10-11",
    "created_at":"1325376000",
    "contact":{
      "id": 99999,
      "full_name":"ACME"
    },
    "country":"US",
    "street_line_1":"Verizon Center",
    "street_line_2":"",
    "city":"Washington DC",
    "region":"Washington DC",
    "postal_code":"",
    "po_number":"",
    "currency":"USD",
    "exchange_rate":"0.680309",
    "items":[
      {
        "id":"48151623423",
        "description":"Rockets",
        "quantity":"50.0",
        "unit_price_cents":"12500",
        "discount_rate":"0.0",
        "tax_1_name":"",
        "tax_1_rate":"",
        "tax_2_name":"",
        "tax_2_rate":"",
        "subtotal_cents":"625000",
        "discount_cents":"0",
        "gross_amount_cents":"625000"
      }
    ],
    "subtotal_cents":"625000",
    "discount_cents":"0",
    "taxes":[],
    "total_cents":"625000",
    "payments":[],
    "tag_list":["rockets", "acme"],
    "notes":"",
    "state":"outstanding",
    "custom_metadata":{}
  },
  {
    "id":"5076a6b92f412e0e2e00016d",
    "issue_date":"2012-10-11",
    "created_at":"1325376020",
    "contact":{
    "id": 99998,
    "full_name":"ACME"
    },
    "country":"US",
    "street_line_1":"Verizon Center",
    "street_line_2":"",
    "city":"Washington DC",
    "region":"Washington DC",
    "postal_code":"",
    "po_number":"",
    "currency":"USD",
    "exchange_rate":"0.680309",
    "items":[
      {
      "id":" 48151623424",
      "description":"TNT",
      "quantity":"100.0",
      "unit_price_cents":"2500",
      "discount_rate":"0.0",
      "tax_1_name":"",
      "tax_1_rate":"",
      "tax_2_name":"",
      "tax_2_rate":"",
      "subtotal_cents":"250000",
      "discount_cents":"0",
      "gross_amount_cents":"250000"
    }
    ],
    "subtotal_cents":"250000",
    "discount_cents":"0",
    "taxes":[],
    "total_cents":"250000",
    "payments":[],
    "tag_list":["tnt", "acme"],
    "notes":"",
    "state":"outstanding",
    "custom_metadata":{}
  }
]
```

#### HTTP Request

`GET /expenses`

#### Parameters

Parameter      | Type      | Description
---------------|-----------|----------------------------------------------------------------------------
`q`            | string    | A case-sensitive filter on the list based on the provide's name or the PO number.
`date`         | string    | A case-sensitive filter on the list based on the invoice's issue date. Pass the date range in the url like `?date=DATE1,DATE2`. Date range allows these formats: `2019-01-01,2019-12-31` or `2019/01/01,2019/12/31`.
`state`        | string    | A case-sensitive filter on the list based on the invoice's state. Can be `outstanding`, `late`, `paid`, and `archived`. 

