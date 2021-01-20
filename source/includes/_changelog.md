# Changelog

## API versions and changes

### 20170914 (Current version)
* Added the accounts API
* Added the `related_document` to the credit object.
* Added `tax_1_transaction_type` and `tax_2_transaction_type` fields to the taxes attributes in the credit object.
* Added `transaction_type` fields to the document items attributes in the credit object.
* Added `kind`, `stripe_plan_id`, `paypal_interval_unit`, `paypal_interval_frequency` and `paypal_interval_duration` fields in the item object.
* Changed `number` field limit to 20 characters for `invoices`, `receipts`, `credits` and `estimates`.

### 20170628
* Return 429 (`too many requests`) instead of 503 (`service unavailable`) on rate limits.

### 20170418
* Added `custom_metadata` as an editable field.

### 20161108
* `country` is now returned as an attribute for `invoices`, `receipts`, `expenses`, `credits` and `estimates`.

### 20160614
* Amounts are now returned as cents instead being formatted as amount with currency symbol.
* Versions are no longer passed as part of the URL, instead they can be passed as an accept header.

### 20160602
* First version.
* Amounts are returned formatted as amount with currency symbol.
