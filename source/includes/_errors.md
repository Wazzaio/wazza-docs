# Errors

Wazza uses conventional HTTP response codes to indicate success or failure of an API request. In general, codes in the 2xx range indicate success, codes in the 4xx range indicate an error that resulted from the provided information (e.g. a required parameter was missing, a charge failed, etc.), and codes in the 5xx range indicate an error with Wazza's servers.
Not all errors map cleanly onto HTTP response codes, however. When a request is valid but does not complete successfully (e.g. a card is declined), we return a 402 error code.

Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is badly formed
401 | Unauthorized -- Your API key is wrong
403 | Forbidden -- You have no permission
404 | Not Found -- The specified resource could not be found
405 | Method Not Allowed -- You tried to access a resource with an invalid method
406 | Not Acceptable -- You requested a format that isn't json
410 | Gone -- The resource requested has been removed from our servers
429 | Too Many Requests -- You're requesting too many resources
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintanance. Please try again later.
