# Errors

We don't usually have any trouble on our end, but when we do we'll let you know!

The Quaderno API uses the following error codes:

Code  | Text                    | Description
------|-------------------------|------------------------------------------------------------------------------------------------------------------------
`400` | `Bad Request`           | Your request may be malformed
`401` | `Unauthorized`          | Your API key is wrong, or your user does not have access to this resource
`403` | `Forbidden`             | The record requested is hidden for administrators only
`404` | `Not Found`             | The specified record could not be found
`405` | `Method Not Allowed`    | You tried to access a record with an invalid method
`406` | `Not Acceptable`        | You requested a format that isn't JSON
`410` | `Gone`                  | The record requested has been removed from our servers
`422` | `Unprocessable Entity`  | The requested method cannot process for the record in question
`429` | `Too Many Requests`     | You're requesting too many records! Slow down!
`500` | `Internal Server Error` | We had a problem with our server. Try again later.
`502` | `Bad Gateway`           | We had a different problem with our server. Try again later.
`503` | `Service Unavailable`   | We're temporarily offline for maintenance, or you've exceeded the [rate limit](#rate-limiting). Please try again later.
`504` | `Gateway Timeout`       | Yep, you guessed it. We'll be back soon!

<aside class="warning">
Note that when API errors occur, it is up to you to retry your request - Quaderno does not keep track of failed calls.<br /><br />

<p>If an error occurs on a call where the service is not down, we will return a JSON response and an error code that we hope will point you to the problem with your call.</p>
</aside>
