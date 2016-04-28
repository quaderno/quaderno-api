# Errors

The Quaderno API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request sucks
401 | Unauthorized -- Your API key is wrong, or your user does not have access to this resource
403 | Forbidden -- The record requested is hidden for administrators only
404 | Not Found -- The specified record could not be found
405 | Method Not Allowed -- You tried to access a record with an invalid method
406 | Not Acceptable -- You requested a format that isn't json
410 | Gone -- The record requested has been removed from our servers
429 | Too Many Requests -- You're requesting too many records! Slow down!
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
