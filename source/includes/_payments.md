## Payments

Payments in Quaderno-lingo represent the recording of a successful payment.

Like [products](#products), payments are a sub-object and can be attached to invoices and expenses. However, payments cannot exist in Quaderno separately to an instance of one of these classes, and the same payment cannot be referenced by more than one instance.

### Create a payment

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/invoices/INVOICE_ID/payments \
  -u YOUR_API_KEY:x \
  -d amount=9.99 \
  -d payment_method="credit_card" \
  -d payment_processor="stripe" \
  -d payment_processor_id="ch_19yUdh2eZvKYlo2CkFVBOZG7" 
```

> The above command returns JSON structured like this:

```json
{
  "id": 46,
  "date": "2021-02-04",
  "payment_method": "cash",
  "amount_cents": 999
}
```

#### HTTP Request

`POST /invoices/INVOICE_ID/payments`

`POST /expenses/EXPENSE_ID/payments`

#### Parameters

Parameter             | Type      | Description
----------------------|-----------|-------------------------------------------------------------------------------------------------------------------------------------
amount                | decimal   | Amount paid. **Required**
date                  | date      | The date of the payment . Defaults to today.
payment_method        | string    | One of the following: `credit_card`, `cash`, `wire_transfer`, `direct_debit`, `check`, `iou`, `paypal` or `other`. **Required**
payment_processor     | string    | Payment processor that you used to process the payment.
payment_processor_id  | string    | The transaction ID that the payment processor assigned to the payment.






### Delete a payment

```shell
curl https://ACCOUNT_NAME.quadernoapp.com/api/invoices/INVOICE_ID/payments/PAYMENT_ID \
  -u YOUR_API_KEY:x \
  -X DELETE
```

#### HTTP Request

`DELETE /invoices/INVOICE_ID/payments/PAYMENT_ID`

`DELETE /expenses/EXPENSE_ID/payments/PAYMENT_ID`

