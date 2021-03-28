## Attachments

Invoices and expenses can have attached files. Each attachment has the following properties:

Parameter               | Type      | Description
------------------------|-----------|--------------------------------------------------------------------------------
`id`                    | integer   | Unique identifier for the object. Available only for updates.
`data`                  | string    | File data enconded in base64. **Required**.
`filename`              | string    | Name of the attached file. **Required**.
`public`                | integer   | Visibility of attached file. Public attachments can be seen and downloaded by customers. Defaults to false.

