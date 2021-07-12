## Credit Notes

A credit note is a reverse credit; something to cancel out - partially or completely - an credit you have issued in the past. It may be for the full amount of an credit or it may be for less in the case of a partial refund.

### Create a credit

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/credits.json \
  -u YOUR_API_KEY:x \
  -d invoice_id=999999
```
> The above command returns JSON structured like this:

```json
{
  "id":"507693322f412e0e2e0000da",
  "number":"0000047",
  "issue_date":"2012-10-11",
  "created_at":"1325376000",
  "related_document":{
    "id":999999,
    "type":"Invoice"
  },
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
  "permalink":"https://my-account.quadernoapp.com/credit/7hef1rs7p3rm4l1nk",
  "pdf":"https://my-account.quadernoapp.com/credit/7hef1rs7p3rm4l1nk.pdf",
  "custom_metadata":{}
}
```

#### HTTP Request

`POST /credits`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`invoice_id`            | integer   | The ID of the refunded invoice. **Required**








### Retrieve a credit

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/credits/CREDIT_ID \
  -u YOUR_API_KEY:x
```
> The above command returns JSON structured like this:

```json
{
  "id":"507693322f412e0e2e0000da",
  "number":"0000047",
  "issue_date":"2012-10-11",
  "created_at":"1325376000",
  "related_document":{
    "id":999999,
    "type":"Invoice"
  },
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
  "permalink":"https://my-account.quadernoapp.com/credit/7hef1rs7p3rm4l1nk",
  "pdf":"https://my-account.quadernoapp.com/credit/7hef1rs7p3rm4l1nk.pdf",
  "custom_metadata":{}
}
```

#### HTTP Request

`GET /credits/CREDIT_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`CREDIT_ID`             | integer   | The ID of the credit to retrieve. **Required**








### Update a credit

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/credits/CREDIT_ID \
  -u YOUR_API_KEY:x \
  -X PUT \
  -d notes="You better pay this time, Tony" \
  -d custom_metadata[memo]="I think he is not paying again."
```

> The above command returns JSON structured like this:

```json
{
  "id":"507693322f412e0e2e0000da",
  "number":"0000047",
  "issue_date":"2012-10-11",
  "created_at":"1325376000",
  "related_document":{
    "id":999999,
    "type":"Invoice"
  },
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
  "permalink":"https://my-account.quadernoapp.com/credit/7hef1rs7p3rm4l1nk",
  "pdf":"https://my-account.quadernoapp.com/credit/7hef1rs7p3rm4l1nk.pdf",
  "custom_metadata":{}
}
```

#### HTTP Request

`PUT /credits/CREDIT_ID`

<aside class="warning">
In our goal to make Quaderno compliant with tax rules worldwide, we're going to limit the edition of credit notes via API as of 1 July.

Only modifiable parameters will be: `po_number`, `tag_list`, `payment_details`, `notes`, `custom_metadata` and all those related to the billing address: `street_line_1`,`street_line_2`,`city`,`region`,and `postal_code`.

You can read more about this breaking change in our guide <a href='#safely-upgrading-to-api-version-20210701'>Safely upgrading to API version 20210701</a>.
</aside>

#### Parameters

Parameter               | Type              | Description
------------------------|-------------------|--------------------------------------------------------------------------------------------------------------
`CREDIT_ID`             | integer           | The ID of the credit to update. **Required**
`number`                | string            | A unique, sequential code that identifies the credit. Legally, an credit number sequence should never contain repeats or gaps. Automatic numbering is used by default.
`issue_date`            | date              | The date of the credit's issue - not necessarily the date the products or services were provided.
`contact`               | object            | The data of the [customer](#contacts) who was refunded.
`street_line_1`         | string            | The customer's billing address line 1 (Street address/PO Box).
`street_line_2`         | string            | The customer's billing address line 2 (Apartment/Suite/Unit/Building).
`city`                  | string            | The customer's billing city.
`region`                | string            | The customer's billing state/province/region.
`postal_code`           | string            | The customer's billing ZIP or postal code. Available for updates.
`country`               | string            | The customer's billing country. 2-letter [ISO country code](https://en.wikipedia.org/wiki/List_of_ISO_3166_country_codes).
`items_attributes`      | array             | Array of [document items](#document-items). **Required**
`payment_processor`     | string            | The name of the payment processor used to refund the payment.
`payment_processor_id`  | string            | The ID of the transaction in the `payment_processor`.
`po_number`             | string            | The number of the related order. 
`tag_list`              | string            | Multiple tags should be separated by commas
`payment_details`       | string            | Detailed information about the accepted payment methods.
`notes`                 | string            | Extra notes about the credit.
`custom_metadata`       | hash        | Set of key-value pairs that you can attach to an object. This can be useful for storing additional information about the object in a structured format. You can have up to 20 keys, with key names up to 40 characters long and values up to 500 characters long.





### Delete a credit

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/credits/CREDIT_ID \
  -u YOUR_API_KEY:x \
  -X DELETE
```
#### HTTP Request

`DELETE /credits/CREDIT_ID`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`CREDIT_ID`             | integer   | The ID of the credit to delete. **Required**








### Deliver an credit

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/credits/CREDIT_ID/deliver \
  -u YOUR_API_KEY:x
```
#### HTTP Request

`GET /credits/CREDIT_ID/deliver`

#### Parameters

Parameter               | Type      | Description
------------------------|-----------|----------------------------------------------------------------------------
`CREDIT_ID`             | integer   | The ID of the credit to deliver. **Required**







### List all credits

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/credits \
  -u YOUR_API_KEY:x
```

```json
[
  {
    "id":"507693322f412e0e2e00000f",
    "number":"0000047",
    "issue_date":"2012-10-11",
    "created_at":"1325376000",
    "related_document":{
      "id":999999,
      "type":"Invoice"
    },
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
        "url":"https://my-account.quadernoapp.com/api/credits/507693322f412e0e2e00000f/payments/50aca7d92f412eda5200002c.json"
      },
    ],
    "payment_details":"Ask Jon",
    "notes":"",
    "state":"paid",
    "tag_list":["lasagna", "cat"],
    "secure_id":"7hef1rs7p3rm4l1nk",
    "permalink":"https://quadernoapp.com/credit/7hef1rs7p3rm4l1nk",
    "pdf":"https://quadernoapp.com/credit/7hef1rs7p3rm4l1nk.pdf",
    "custom_metadata":{}
  },

  {
    "id":"507693322f412e0e2e0000da",
    "number":"0000047",
    "issue_date":"2012-10-11",
    "created_at":"1325376020",
    "related_document":{
      "id":875978,
      "type":"Invoice"
    },
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
    "permalink":"https://my-account.quadernoapp.com/credit/7hes3c0ndp3rm4l1nk",
    "pdf":"https://my-account.quadernoapp.com/credit/7hes3c0ndp3rm4l1nk.pdf",
    "custom_metadata":{}
  }
]
```

#### HTTP Request

`GET /credits`

#### Parameters

Parameter      | Type      | Description
---------------|-----------|----------------------------------------------------------------------------
`q`            | string    | A case-sensitive filter on the list based on the invoice's number or customer name.
`date`         | string    | A case-sensitive filter on the list based on the invoice's issue date. Pass the date range in the url like `?date=DATE1,DATE2`. Date range allows these formats: `2019-01-01,2019-12-31` or `2019/01/01,2019/12/31`.
`state`        | string    | A case-sensitive filter on the list based on the invoice's state. Can be `outstanding`, `late`, `paid`, `refunded` and `archived`. 


