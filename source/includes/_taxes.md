# Taxes

One of the killer features Quaderno provides is efficient and easy tax management. As part of this, we offer an API for easy tax calculation.

## Calculate Taxes

> `GET /taxes/calculate.json?country=ES&postal_code=08080&vat_number=ESA58818501`

```json
{
    "name": "VAT",
    "rate": 21.0,
    "notes": null
}
```

`GET`ting to `/taxes/calculate.json` will calculate the applicable taxes given a customer's data,

Parameter | Mandatory | Description
---|---|---
`country` | **Yes** | Customer's country (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes))
`postal_code` | No | Customer's postal code (ZIP)
`vat_number` | No | Customer's VAT number
`transaction_type` | No | Values: `eservice`, `ebook`, `standard`. Default is `eservice`

This will return a `200 OK` if the request was a success, along with the taxes represented as a JSON string.
