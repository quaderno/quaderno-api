## Events 

Events are our way of letting you know when something interesting happens in your account. When an interesting event occurs, we create a new `Event` object. For example, when a new invoice is created, we create a `invoice.created` event; and when an invoice is deleted, we create an `invoice.deleted` event.


### The event object 

```json
{
  "event_type":"invoice.created",
  "account_id": 99999,
  "data":
  {
    "object":
    {
      "id":925,
      "contact_id":128,
      "tag_list":[],
      "number":"123346",
      "issue_date":"2016-02-12",
      "contact_name":"OrsonFarm",
      "contact": {
        "first_name":"OrsonFarm",
        "last_name":null,
        "email":"orso@farm.com",
        "contact_person":"Orson Welles"
      },
      "currency":"EUR",
      "gross_amount_cents":826,
      "total_cents":1000,
      "amount_paid_cents":0,
      "po_number":null,
      "payment_details":null,
      "notes":null,
      "state":"outstanding",
      "subject":null,
      "street_line_1":null,
      "street_line_2":null,
      "city":null,
      "postal_code":null,
      "region":null,
      "country":"ES",
      "processor_id":null,
      "processor":null,
      "processor_fee_cents":null,
      "custom_metadata": {
        "my_custom_id": "123456"
      },
      "due_date":null,
      "permalink":"5191567e45d6aa419927ce7a8ba2ee870c9f0a45",
      "email":null,
      "gross_amount":"8.26",
      "total":"10.00",
      "amount_paid":"0.00",
      "items":
      [
        {
          "id":1056,
          "description":"Test item acces token",
          "quantity":"1.0",
          "unit_price":"8.26",
          "discount_rate":"0.0",
          "tax_1_amount_cents":174,
          "tax_1_name":"IVA",
          "tax_1_rate":21.0,
          "tax_1_country":null,
          "tax_2_amount_cents":0,
          "tax_2_name":null,
          "tax_2_rate":null,
          "tax_2_country":null,
          "subtotal_cents":"826.0",
          "total_amount_cents":1000,
          "discount_cents":"0.0",
          "taxes_included":true,
          "reference":null,
          "tax_1_amount":"1.74",
          "tax_2_amount":"0.00",
          "unit_price_cents":"826.00",
          "discount":"0.00",
          "subtotal":"8.26",
          "total_amount":"10.00"
        }
      ],
      "payments":[]
    }
  }
}

{
  "event_type":"contact.updated",
  "account_id": 99999,
  "data":
  {
    "object":
    {
      "id":76,
      "kind":"person",
      "first_name":"Adella",
      "last_name":"Schowalter",
      "full_name":"Adella Schowalter",
      "contact_name":null,
      "street_line_1":null,
      "street_line_2":null,
      "postal_code":null,
      "city":null,
      "region":null,
      "country":"DE",
      "phone_1":null,
      "email":null,
      "web":null,
      "discount":null,
      "language":"EN",
      "tax_id":null,
      "currency":"USD",
      "notes":null,
      "processor_id":null,
      "processor":null
    }
  }
}

{
  "event_type":"payment.created",
  "account_id": 99999,
  "data":
  {
    "object":
    {
      "id":15,
      "document_id":815,
      "date":"2015-09-22",
      "payment_method":"credit_card",
      "amount_cents":400,
      "amount":"4.00"
    }
  }
}

{
  "event_type":"checkout.succeeded",
  "account_id": 99999,
  "data":{
    "object":{
      "transaction_details":{
        "session":43,
        "session_permalink":"https://demo.quadernoapp.com/checkout/session/8ccf3fdc42b85800188b113b81d3e4212ef094b3",
        "gateway":"stripe",
        "type":"charge",
        "description":"Unicorn",
        "customer":"cus_FXesSyaK3CG8Oz",
        "email":"john@doe.com",
        "transaction":"pi_1F2ZYtEjVHvINKlcq2as8H5V",
        "product_id":"prod_61ffa845b4a0b8",
        "tax_name":"VAT",
        "tax_rate":20.0,
        "extra_tax_name":null,
        "extra_tax_rate":null,
        "iat":1564647784,
        "amount_cents":1500,
        "amount":"15.00",
        "currency":"EUR"
      },
      "contact":{
        "id":547540,
        "kind":"company",
        "first_name":"John ",
        "last_name":"Doe",
        "full_name":"John  Doe",
        "contact_name":null,
        "street_line_1":"Fake Street 1",
        "postal_code":"SW15 5PU",
        "city":null,
        "region":null,
        "country":"GB",
        "email":"john@doe.com",
        "web":null,
        "language":"EN",
        "tax_id":null,
      }
    }
 }
}

{
  "event_type":"checkout.failed",
  "account_id": 99999,
  "data":{
    "object":{
      "message":{
        "response_message":"Insufficient funds",
        "status_code":422
      },
      "transaction_details":{
        "gateway":"stripe",
        "type":"charge",
        "description":"Unicorn",
      },
      "contact":{
        "id":547540,
        "kind":"company",
        "first_name":"John ",
        "last_name":"Doe",
        "full_name":"John  Doe",
        "contact_name":null,
        "street_line_1":"Fake Street 1",
        "postal_code":"SW15 5PU",
        "city":null,
        "region":null,
        "country":"GB",
        "email":"john@doe.com",
        "web":null,
        "language":"EN",
        "tax_id":null,
      }
    }
  }
}

{
  "event_type":"checkout.abandoned",
  "account_id": 99999,
  "data":{
    "object":{
      "transaction_details":{
        "description":"Unicorn",
        "plan":"awesome"
      },
      "contact":{
        "first_name":"John ",
        "last_name":"Doe",
        "city":null,
        "country":"GB",
        "email":"john@doe.com"
      }
    }
  }
}
```

All event uses the same data format, regardless of event type:

Parameter    | Type     |  Description
-------------|----------|--------------------------------------------------------------------------------------------
`event_type` | string   | The event which triggered the webhook (`invoice.created`, `contact.deleted`, `payment.created`, etc).
`account_id` | integer  | ID of the account that originated the event.
`data`       | hash     | A simplified JSON representation of the object.





### Types of events

This is a list of all the types of events we currently send. We may add more at any time, so in developing and maintaining your code, you should not assume that only these types exist.

You'll notice that these events follow a pattern: `resource.event`. Our goal is to design a consistent system that makes things easier to anticipate and code against.

Event types are a combination of the object you want to be notified about and the object state.

Available events are:

- `checkout.succeeded` – Occurs when a checkout session has been successfully completed.
- `checkout.failed` – Occurs when a checkout session fails.
- `checkout.abandoned` – Occurs when a checkout session is abandoned by the customer.
- `contact.created` – Occurs whenever a new contact is created.
- `contact.updated` – Occurs whenever any property of a contact changes.
- `contact.deleted` – Occurs whenever a contact is deleted.
- `credit.created` – Occurs whenever a new credit note is created.
- `credit.updated` – Occurs whenever a credit note changes (e.g., the credit amount).
- `credit.deleted` – Occurs whenever a credit note is deleted.
- `expense.created` – Occurs whenever a new expense is created.
- `expense.updated` – Occurs whenever an expense changes (e.g., the expense amount).
- `expense.deleted` – Occurs whenever an expense is deleted.
- `invoice.created` – Occurs whenever a new invoice is created.
- `invoice.updated` – Occurs whenever an invoice changes (e.g., the invoice amount).
- `invoice.deleted` – Occurs whenever an invoice is deleted.
- `payment.created` – Occurs whenever a new payment is created.
- `payment.deleted` – Occurs whenever a payment is deleted.
- `reporting.request.suceeded` – Occurs whenever a requested report completed succesfully.
- `reporting.request.failed`  – Occurs whenever a requested report failed to complete.






### Endpoint errors

If your event-receiving endpoint does not respond with a `200` when Quaderno `POST`s the data, Quaderno will try again within 48 hours.

If you fail to respond a second time then your subscription to that event will be deleted.

<aside class="notice">
  We recommend responding with a status code between 200 and 299 (inclusive) so that Quaderno does not assume there is an error in your endpoint.
</aside>







### Checking the webhook signatures

Quaderno signs all webhook events it sends to your endpoints by including a signature in each event’s `X-Quaderno-Signature` header. This allows you to verify that requests were sent by Quaderno and not by a third-party impersonating us.

The key is returned when you [create a webhook](#create-a-webhook) or when you [list all your webhooks](#list-all-webhooks). 

To verify a request, you must generate a signature using the same secret key used by Quaderno and comparing the result to the value of the `X-Quaderno-Signature` header.

In your code that processes webhooks:

1. Create a string with the webhook's URL, exactly as you entered it in Quaderno (including any query string, if applicable). Quaderno always signs webhook requests with the exact URL you provided when you configured the webhook. A difference as small as including or removing a trailing slash will prevent the signature from validating.
2. Append the `POST` body to the URL string, with no delimiter. The body should be a JSON-encoded representation of the data.
3. Hash the resulting string with `HMAC-SHA1`, using your webhook's authentication key to generate a binary signature.
4. Base64 encode the binary signature.
5. Compare the binary signature that you generated to the signature provided in the `X-Quaderno-Signature` HTTP header.

