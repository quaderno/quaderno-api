# TRANSACTIONS

Create a `Transaction` object to easily send sales & refunds from your platform to Quaderno. We'll use them to issue invoices & credit notes, as well as update tax reports automatically.

The Transactions API is perfect for use cases where your customers pay you at the moment and for using along Quaderno Connect. With Transactions API, you'll be able to issue the invoice with the payment information and create the contact or reference the existing contact in just one performant API call.

## Create a transaction

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/transactions \
  -u YOUR_API_KEY:x \
  -d type="sale" \
  -d currency="USD" \
  -d customer[id]=456987213 \ 
  -d items[0][description]="Pizza" \
  -d items[0][amount]=10 \
  -d payment[method]="credit_card" \ 
  -d payment[processor]="stripe" \
  -d payment[processor_id]="ch_1IMFwhB2xq6voISLLk4I1KeE"
  ...
```

> The above command returns JSON structured like this:

```json
{
  "id": 507693322,
  "contact": {
    "id": 456987213,
    "city": "Pasadena",
    "country": "US",
    "created_at": 1614009776,
    "email": "s.cooperphd@yahoo.com",
    "first_name": "Sheldon",
    "full_name": "Sheldon Cooper",
    "kind": "person",
    "language": "EN",
    "last_name": "Cooper",
    "notes": null,
    "phone_1": null,
    "postal_code": "91104",
    "region": "CA",
    "street_line_1": "2311 N. Los Robles Avenue",
    "street_line_2": null,
    "tax_id": null,
    "web": null
  },
  "created_at": 1614105570,
  "currency": "USD",
  "discount_cents": 0,
  "items": [
    {
      "id": 94,
      "created_at": 1614105570,
      "description": "Pizza",
      "discount_cents": 0,
      "discount_rate": 0.0,
      "product_code": null,
      "quantity": 1.0,
      "subtotal_cents": 907,
      "tax_code": "standard",
      "tax_country": "US",
      "tax_name": "Sales tax",
      "tax_rate": 10.25,
      "tax_region": "CA",
      "taxable_part": "100.0",
      "total_amount_cents": 1000,
      "unit_price": 9.07
    }
  ],
  "payments": [
    {
      "id": 47,
      "amount_cents": 1000,
      "created_at": 1614105571,
      "date": "2021-02-23",
      "payment_method": "credit_card"
    }
  ],
  "state": "paid",
  "subtotal_cents": 6160,
  "taxes": [
    {
      "amount_cents": 93,
      "country": "US",
      "label": "Sales tax (10.25%)",
      "rate": 10.25,
      "region": "CA",
      "tax_code": "standard"
    }
  ],
  "total_cents": 1000,
  "type": "Invoice"
}
```

#### HTTP Request

`POST /transactions`

#### Parameters

**Transaction parameters:**

Parameter               | Type              | Description
------------------------|-------------------|--------------------------------------------------------------------------------
`type`                  | string            | The transaction's type. Can be `sale` and `refund`. Defaults to `sale`.
`date`                  | date              | The transaction's date. Defaults to today.
`customer`              | object            | The data of the [customer](#contacts) who paid the transaction.
`shipping_address`      | object            | The mailing address to where the order will be shipped. Use it if the order contains physical goods.  
`currency`              | string            | Three-letter [ISO currency code](https://en.wikipedia.org/wiki/ISO_4217), in uppercase. Defaults to the account's default currency.
`items`                 | array             | The individual items that make up the transaction. See parameters below. **Required**
`payment`               | object            | Detailed information about the transaction payment. See parameters below. **Required**
`processor`             | string            | The name of the platform that processed the transaction. E.g. shopify, woocommerce… 
`processor_id`          | string            | The ID of the transaction in the `processor`.
`evidence`              | object            | Location evidence of the customer's residence. See parameters below.
`po_number`             | string            | The number of the related order. 
`tags`                  | string            | Tags attached to the transaction, formatted as a string of comma-separated values. Tags are additional short descriptors, commonly used for filtering and searching. Each individual tag is limited to 40 characters in length.
`notes`                 | string            | Optional notes attached to the transaction.
`custom_metadata`       | hash              | Set of key-value pairs that you can attach to an object. This can be useful for storing additional information about the object in a structured format. You can have up to 20 keys, with key names up to 40 characters long and values up to 500 characters long.


**Transaction item parameters:**

Parameter               | Type              | Description
------------------------|-------------------|--------------------------------------------------------------------------------
`product_code`          | string            | Code of an existing [product](#products). If present, you don't need to set the description.
`description`           | string            | An arbitrary string attached to the object. Often useful for displaying to users. **Required**.
`quantity`              | integer           | Quantity of units for the transaction item. Defaults to 1.
`discount_rate`         | integer           | This represents the discount percent out of 100 included in the `amount`, if applicable.
`tax`                   | object            | The tax rates which apply to the transaction item. Use the response from the [tax calculation endpoint](#calculate-a-tax-rate).
`amount`                | decimal           | Total amount (in the `currency` specified) of the transaction item. This should always be equal to the amount charged after discounts and taxes. **Required**.

**Transaction payment parameters:**

Parameter               | Type              | Description
------------------------|-------------------|--------------------------------------------------------------------------------
`method`                | string            | The payment method used to pay the transaction. Can be `credit_card`, `cash`, `wire_transfer`, `direct_debit`, `check`, `iou`, `paypal` or `other` Defaults to `other`.
`processor`             | string            | The name of the payment processor used to take the payment. E.g. stripe, paypal…
`processor_id`          | string            | The ID of the transaction in the `processor`.

**Transaction evidence parameters:**

Parameter               | Type              | Description
------------------------|-------------------|--------------------------------------------------------------------------------
`billing_country`       | string            | Customer's billing country (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
`ip_address`            | string            | Customer's IP address
`bank_country`          | string            | Customer's bank country (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
