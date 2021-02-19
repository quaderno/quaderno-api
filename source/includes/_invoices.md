# BILLING 

## Invoices

An invoice is a detailed list of goods shipped or services rendered, with an account of all costs.

### Create an invoice

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/invoices \
  -u YOUR_API_KEY:x \
  -d contact[id]=999999 \
  -d contact[first_name]="Teenage Mutant Ninja Turtles" \
  -d currency="USD" \
  -d items_attributes[0][description]="Pizza" \
  -d items_attributes[0][quantity]=5 \
  -d items_attributes[0][total_amount]=60 \
  -d custom_metadata[a_custom_key]="a custom value"
  ...
```

> The above command returns JSON structured like this:

```json
{
  "id":"507693322f412e0e2e0000da",
  "number":"0000047",
  "issue_date":"2012-10-11",
  "contact":{
    "id":"999999",
    "full_name":"Teenage Mutant Ninja Turtles"
  },
  "country":"US",
  "street_line_1":"Melrose Ave, Sewer #3",
  "street_line_2":"",
  "city":"New York",
  "region":"New York",
  "postal_code":"",
  "po_number":null,
  "due_date":null,
  "currency":"USD",
  "exchange_rate":"0.680309",
  "items":[
    {
      "id":"48151623429",
      "description":"Pizza",
      "quantity":"15.0",
      "unit_price_cents":"6000",
      "discount_rate":"0.0",
      "tax_1_name":"",
      "tax_1_rate":"",
      "tax_2_name":"",
      "tax_2_rate":"",
      "subtotal_cents":"6000",
      "discount_cents":"0",
      "gross_amount_cents":"6000"
    }
  ],
  "subtotal_cents":"6000",
  "discount_cents":"0",
  "taxes":[],
  "total_cents":"6000",
  "payments":[],
  "payment_details":"",
  "notes":"",
  "state":"outstanding",
  "tag_list":["lasagna", "cat"],
  "secure_id":"7hef1rs7p3rm4l1nk",
  "permalink":"https://ACCOUNT_NAME.quadernoapp.com/invoice/7hef1rs7p3rm4l1nk",
  "pdf":"https://ACCOUNT_NAME.quadernoapp.com/invoice/7hef1rs7p3rm4l1nk.pdf",
  "custom_metadata":{}
}
```

#### HTTP Request

`POST /invoices`

#### Parameters

Parameter               | Type              | Description
------------------------|-------------------|--------------------------------------------------------------------------------
`number`                | string            | A unique, sequential code that identifies the invoice. Legally, an invoice number sequence should never contain repeats or gaps. Automatic numbering is used by default.
`issue_date`            | date              | The date of the invoice's issue – not necessarily the date the products or services were provided.
`currency`              | string            | Three-letter [ISO currency code](https://en.wikipedia.org/wiki/ISO_4217), in uppercase. Defaults to the account's default currency.
`contact`               | object            | The data of the [customer](/#contacts) who will be billed.
`items_attributes`      | array             | Array of [document items](/#document-items). **Required**
`payment_method`        | string            | Use this parameter to mark this document as paid in a single request. Can be `credit_card`, `cash`, `wire_transfer`, `direct_debit`, `check`, `iou`, `paypal` or `other`
`payment_processor`     | string            | The name of the payment processor used to take the payment.
`payment_processor_id`  | string            | The ID of the transaction in the `payment_processor`.
`po_number`             | string            | The number of the related order. 
`tag_list`              | string            | Multiple tags should be separated by commas
`payment_details`       | string            | Detailed information about the accepted payment methods.
`notes`                 | string            | Extra notes about the invoice.
`custom_metadata`       | hash        | Set of key-value pairs that you can attach to an object. This can be useful for storing additional information about the object in a structured format. You can have up to 20 keys, with key names up to 40 characters long and values up to 500 characters long.




### Retrieve an invoice

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/invoices/INVOICE_ID \
  -u YOUR_API_KEY:x
```
> The above command returns JSON structured like this:

```json
{
  "id":"507693322f412e0e2e0000da",
  "number":"0000047",
  "issue_date":"2012-10-11",
  "contact":{
    "id":"5073f9c22f412e02d00004cf",
    "full_name":"Teenage Mutant Ninja Turtles"
  },
  "country":"US",
  "street_line_1":"Melrose Ave, Sewer #3",
  "street_line_2":"",
  "city":"New York",
  "region":"New York",
  "postal_code":"",
  "po_number":null,
  "due_date":null,
  "currency":"USD",
  "exchange_rate":"0.680309",
  "items":[
    {
      "id":"48151623429",
      "description":"pizza",
      "quantity":"15.0",
      "unit_price_cents":"6000",
      "discount_rate":"0.0",
      "tax_1_name":"",
      "tax_1_rate":"",
      "tax_2_name":"",
      "tax_2_rate":"",
      "reference":"Even the bad ones taste good!",
      "subtotal_cents":"6000",
      "discount_cents":"0",
      "gross_amount_cents":"6000"
    }
  ],
  "subtotal_cents":"6000",
  "discount_cents":"0",
  "taxes":[],
  "total_cents":"6000",
  "payments":[],
  "payment_details":"",
  "notes":"",
  "state":"outstanding",
  "tag_list":["lasagna", "cat"],
  "secure_id":"7hef1rs7p3rm4l1nk",
  "permalink":"https://ACCOUNT_NAME.quadernoapp.com/invoice/7hef1rs7p3rm4l1nk",
  "pdf":"https://ACCOUNT_NAME.quadernoapp.com/invoice/7hef1rs7p3rm4l1nk.pdf",
  "custom_metadata":{}
}
```

#### HTTP Request

`GET /invoices/INVOICE_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`INVOICE_ID`            | integer   | The ID of the invoice to retrieve. **Required**





### Update an invoice

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/invoices \
  -u YOUR_API_KEY:x \
  -X PUT \
  -d currency="USD" \
  -d items_attributes[0][description]="Pizza" \
  -d items_attributes[0][quantity]=10 \
  -d items_attributes[0][total_amount]=75 \
  ...
```
> The above command returns JSON structured like this:

```json
{
  "id":"507693322f412e0e2e0000da",
  "number":"0000047",
  "issue_date":"2012-10-11",
  "contact":{
    "id":"5073f9c22f412e02d00004cf",
    "full_name":"Teenage Mutant Ninja Turtles"
  },
  "country":"US",
  "street_line_1":"Melrose Ave, Sewer #3",
  "street_line_2":"",
  "city":"New York",
  "region":"New York",
  "postal_code":"",
  "po_number":null,
  "due_date":null,
  "currency":"USD",
  "exchange_rate":"0.680309",
  "items":[
    {
      "id":"48151623429",
      "description":"pizza",
      "quantity":"15.0",
      "unit_price_cents":"6000",
      "discount_rate":"0.0",
      "tax_1_name":"",
      "tax_1_rate":"",
      "tax_2_name":"",
      "tax_2_rate":"",
      "reference":"Even the bad ones taste good!",
      "subtotal_cents":"6000",
      "discount_cents":"0",
      "gross_amount_cents":"6000"
    }
  ],
  "subtotal_cents":"6000",
  "discount_cents":"0",
  "taxes":[],
  "total_cents":"6000",
  "payments":[],
  "payment_details":"",
  "notes":"",
  "state":"outstanding",
  "tag_list":["lasagna", "cat"],
  "secure_id":"7hef1rs7p3rm4l1nk",
  "permalink":"https://ACCOUNT_NAME.quadernoapp.com/invoice/7hef1rs7p3rm4l1nk",
  "pdf":"https://ACCOUNT_NAME.quadernoapp.com/invoice/7hef1rs7p3rm4l1nk.pdf",
  "custom_metadata":{}
}
```
#### HTTP Request

`PUT /invoices/INVOICE_ID`

#### Parameters

Parameter               | Type              | Description
------------------------|-------------------|--------------------------------------------------------------------------------------------------------------
`INVOICE_ID`            | integer           | The ID of the invoice to update. **Required**
`number`                | string            | A unique, sequential code that identifies the invoice. Legally, an invoice number sequence should never contain repeats or gaps. Automatic numbering is used by default.
`issue_date`            | date              | The date of the invoice's issue - not necessarily the date the products or services were provided.
`contact`               | object            | The data of the [customer](/#contacts) who will be billed.
`street_line_1`         | string            | The customer's billing address line 1 (Street address/PO Box).
`street_line_2`         | string            | The customer's billing address line 2 (Apartment/Suite/Unit/Building).
`city`                  | string            | The customer's billing city.
`region`                | string            | The customer's billing state/province/region.
`postal_code`           | string            | The customer's billing ZIP or postal code. Available for updates.
`country`               | string            | The customer's billing country. 2-letter [ISO country code](https://en.wikipedia.org/wiki/List_of_ISO_3166_country_codes).
`items_attributes`      | array             | Array of [document items](/#document-items). **Required**
`payment_processor`     | string            | The name of the payment processor used to take the payment.
`payment_processor_id`  | string            | The ID of the transaction in the `payment_processor`.
`po_number`             | string            | The number of the related order. 
`tag_list`              | string            | Multiple tags should be separated by commas
`payment_details`       | string            | Detailed information about the accepted payment methods.
`notes`                 | string            | Extra notes about the invoice.
`custom_metadata`       | hash        | Set of key-value pairs that you can attach to an object. This can be useful for storing additional information about the object in a structured format. You can have up to 20 keys, with key names up to 40 characters long and values up to 500 characters long.






### Delete an invoice

We don't recommend deleting an invoice to avoid issues with tax authorities. If you want to cancel an invoice, you can convert it into a credit note.

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/invoices/INVOICE_ID \
  -u YOUR_API_KEY:x
```
#### HTTP Request

`DELETE /invoices/INVOICE_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`INVOICE_ID`            | integer   | The ID of the invoice to delete. **Required**








### Deliver an invoice

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/invoices/INVOICE_ID/deliver \
  -u YOUR_API_KEY:x
```
#### HTTP Request

`GET /invoices/INVOICE_ID/deliver`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`INVOICE_ID`            | integer   | The ID of the invoice to deliver. **Required**






### List all invoices

```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/invoices.json'
```
> The above command returns JSON structured like this:

```json
[
  {
    "id":"507693322f412e0e2e00000f",
    "number":"0000047",
    "issue_date":"2012-10-11",
    "created_at":"1325376000",
    "contact":{
      "id":"5073f9c22f412e02d0000032",
      "full_name":"Garfield"
    },
    "country":"US",
    "street_line_1":"Street 23",
    "street_line_2":"",
    "city":"New York",
    "region":"New York",
    "postal_code":"10203",
    "po_number":null,
    "due_date":null,
    "currency":"USD",
    "exchange_rate":"0.680309",
    "items":[
      {
        "id":"48151623429",
        "description":"lasagna",
        "quantity":"25.0",
        "unit_price_cents":"375",
        "discount_rate":"0.0",
        "tax_1_name":"",
        "tax_1_rate":"",
        "tax_2_name":"",
        "tax_2_rate":"",
        "reference":"Awesome!",
        "subtotal_cents":"9375",
        "discount_cents":"0",
        "gross_amount_cents":"9375"
      }
    ],
    "subtotal_cents":"9375",
    "discount_cents":"0",
    "taxes":[],
    "total_cents":"9375",
    "payments":[
      {
        "id":"50aca7d92f412eda5200002c",
        "date":"2012-11-21",
        "payment_method":"credit_card",
        "payment_processor":"stripe",
        "payment_processor_id":"ch_19yUdh2eZvKYlo2CkFVBOZG7",
        "amount_cents":"9375",
      },
    ],
    "payment_details":"Ask Jon",
    "notes":"",
    "state":"paid",
    "tag_list":["lasagna", "cat"],
    "secure_id":"7hef1rs7p3rm4l1nk",
    "permalink":"https://quadernoapp.com/invoice/7hef1rs7p3rm4l1nk",
    "pdf":"https://quadernoapp.com/invoice/7hef1rs7p3rm4l1nk.pdf",
    "custom_metadata":{}
  },

  {
    "id":"507693322f412e0e2e0000da",
    "number":"0000047",
    "issue_date":"2012-10-11",
    "created_at":"1325376020",
    "contact":{
      "id":"5073f9c22f412e02d00004cf",
      "full_name":"Teenage Mutant Ninja Turtles"
    },
    "country":"US",
    "street_line_1":"Melrose Ave, Sewer #3",
    "street_line_2":"",
    "city":"New York",
    "region":"New York",
    "postal_code":"",
    "po_number":null,
    "due_date":null,
    "currency":"USD",
    "exchange_rate":"0.680309",
    "items":[
      {
        "id":"481516234291",
        "description":"pizza",
        "quantity":"15.0",
        "unit_price_cents":"6000",
        "discount_rate":"0.0",
        "tax_1_name":"",
        "tax_1_rate":"",
        "tax_2_name":"",
        "tax_2_rate":"",
        "reference":"Even the bad ones taste good!",
        "subtotal_cents":"6000",
        "discount_cents":"0",
        "gross_amount_cents":"6000"
      }
    ],
    "subtotal_cents":"6000",
    "discount_cents":"0",
    "taxes":[],
    "total_cents":"6000",
    "payments":[],
    "payment_details":"",
    "notes":"",
    "state":"outstanding",
    "tag_list":["pizza", "turtles"],
    "secure_id":"7hes3c0ndp3rm4l1nk",
    "permalink":"https://ACCOUNT_NAME.quadernoapp.com/invoice/7hes3c0ndp3rm4l1nk",
    "pdf":"https://ACCOUNT_NAME.quadernoapp.com/invoice/7hes3c0ndp3rm4l1nk.pdf",
    "custom_metadata":{}
  }
]
```

#### HTTP Request

`GET /invoices`

#### Parameters

Parameter      | Type      | Description
---------------|-----------|----------------------------------------------------------------------------
`q`            | string    | A case-sensitive filter on the list based on the invoice's number or customer name.
`date`         | string    | A case-sensitive filter on the list based on the invoice's issue date. Pass the date range in the url like `?date=DATE1,DATE2`. Date range allows these formats: `2019-01-01,2019-12-31` or `2019/01/01,2019/12/31`.
`state`        | string    | A case-sensitive filter on the list based on the invoice's state. Can be `outstanding`, `late`, `paid`, `refunded` and `archived`. 
