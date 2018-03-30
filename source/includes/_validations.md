# Email Validations API

The following APIs are helpful for ensuring the integrity of your email lists,
outbound mail, and inbound mail.

## Validate Email Address Integrity
### `GET /api/validations/addresses/:addressToValidate`
### `POST /api/validations/addresses`

> Example Request

```bash
POST /api/validations/addresses
{
    "emails": ["greg@reconmail.com", "greg@mailsac.com"]
}
```

> Example Response

```json
[{
    "email": "greg@reconmail.com",
    "validFormat": true,
    "local": "greg",
    "domain": "reconmail.com",
    "isDisposable": true,
    "disposableDomains": ["mailinator.com"],
    "aliases": ["23.239.11.30", "mailinator.com"]
}, {
    "email": "greg@mailsac.com",
    "validFormat": true,
    "local": "greg",
    "domain": "mailsac.com",
    "isDisposable": true,
    "disposableDomains": ["sanstr.com", "mailsac.com", "totalvista.com"],
    "aliases":["sanstr.com", "45.76.28.175", "mailsac.com", "totalvista.com"]
}]
```

Determine whether an email address is a valid format, whether it
is a diposable address, and the domains or IP addresses it is
associated with.

There are two routes for validating email addresses.

The GET route is for quickly testing in a web browser, or for testing
one email address at a time.

The POST route accepts an array of up to 50 email addresses.

**Both** routes return an array, but in the case of the GET route,
there will be only one result.

### POST Body

Field | Description
------|-------------
emails | array of string email addresses

### Response Body Objects

Field | Description
------|-------------
email | string email address that was checked
validFormat | Boolean indicating if the format is valid
local | the "local part" of the email address
domain | the domain of the email address which was used for evaluating disposable components
isDisposable | Boolean indicating if this email address is known to resolve to disposable email providers, most likely making it not useful for marketing mailing lists or signups.
disposableDomains | Array of string domains where this email resolves to, which is helpful when the domain is custom, but receives its mail at a disposable email provider.
aliases | Array of string domains and IP addresses which are associated with the domain for this email address.
