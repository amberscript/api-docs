# Errors

Amberscript API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid. Please check your request body and/or parameters.
401 | Unauthorized -- Your API key is invalid.
403 | Forbidden -- You do have access to requested resource.
404 | Not Found -- The requested resource could not be found.
405 | Method Not Allowed -- You tried to access a resource with an invalid method.
406 | Not Acceptable -- You requested a format that isn't json.
410 | Gone -- The resource requested has been removed from our servers.
429 | Too Many Requests -- Rate limit reached
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
