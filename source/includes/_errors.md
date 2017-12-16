# API Errors

## JSON Error Formats

```json
    {
        "error": "error info here",
        "message": "or a message here"
    }
```

The Mailsac API attempts to send errors in a standardized format using
HTTP status codes.

## Error Codes

HTTP Status | Status Message | Meaning
-----------|----------------|--------
400 | Bad Request |  Fix the validation error and try the request again
401 | Unauthorized | Your API key is wrong or you are requesting something that belongs to someone else
403 | Forbidden | No matter how you repeat the request, it will not succeed with those credentials
404 | Not Found | The requested resource does not exist, or no longer exists, or the URL route is wrong
418 | I'm a teapot | Short and stout
420 | Relax your calm | Chill
429 | Too Many Requests | Please slow down your requests
500 | Internal Server Error | We had a problem with our server and have been notified via our monitoring - try again later
503 | Service Unavailable | We're temporarially offline for maintanance - try again later
