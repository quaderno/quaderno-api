## Document Items

Invoices, credits, and expenses are made up of one or more document items. Each document item has the following properties:

Parameter                 | Type      | Description
--------------------------|-----------|--------------------------------------------------------------------------------
`id`                      | integer   | Unique identifier for the object. Available only for updates
`description`             | string    | An arbitrary string attached to the object. Often useful for displaying to users. **Required**
`quantity`                | decimal   | Quantity of units for the document item. Defaults to 1.
`unit_price`              | decimal   | The unit price of the item before any discount or tax is applied. Required if `total_amount` is not passed.
`total_amount`            | decimal   | The total amount to be charged after discounts and taxes. Required if `unit_price` is not passed.
`discount_rate`           | decimal   | This represents the discount percent out of 100, if applicable.
`tax_1_name`              | string    | The name of the tax, if applicable. Required if `tax_1_rate` is present
`tax_1_rate`              | decimal   | The rate of the tax, if applicable. Required if `tax_1_name` is present
`tax_1_country`           | string    | Tax country, if applicable. Defaults to the contact's country
`tax_1_region`            | string    | Tax region, if applicable. Only for US sales tax and Canada GST.
`tax_1_transaction_type`  | string    | Type of transaction. Can be any [`tax_code`](#list-all-tax-codes). Defaults to the account's default tax class.
`tax_2_name`              | string    | The name of the additional tax, if applicable. Required if `tax_2_rate` is present
`tax_2_rate`              | decimal   | The rate of the additional tax, if applicable. Required if `tax_2_name` is present
`tax_2_country`           | string    | Tax country, if applicable. Defaults to the contact's country
`tax_2_region`            | string    | Tax region, if applicable. Only for US sales tax and Canada GST.
`tax_2_transaction_type`  | string    | Type of transaction. Can be any [`tax_code`](#list-all-tax-codes). Defaults to the account's default tax class.
`reference`               | string    | Code of an existing product. **If present none of the mandatory attributes are required (as you are referencing an item that already exists)**
`_destroy`                | integer   | Set it to 1 if you want to remove the document item selected by ID. Available only for updates.

<aside class="notice">
  No more than 200 document items are allowed in a API request. To add more use subsequent update requests. Maximum items per document are limited up to 1000.
</aside>