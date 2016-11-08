# Changelog

## API versions and changes

### 20161108 (Current version)
* `country` is now returned as an attribute for `invoices`, `receipts`, `expenses`, `credits` and `estimates`

### 20160614
* Amounts are now returned as cents instead being formatted as amount with currency symbol.
* Versions are no longer passed as part of the URL, instead they can be passed as an accept header.

### 20160602

* First version.
* Amounts are returned formatted as amount with currency symbol.
