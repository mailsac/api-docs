# Authentication

API keys are used to auth with the API. They can be passed as a header, body
parameter, or querystring parameter.

## API Key Format

> Example Key

```bash
eoj1mn7x5y61w0egs25j6xrvs6lwrrld0oh43rj583cgdps10tokp2ceux9s6ri8
```

API keys are alphanumeric, cryptographically random tokens.

## Get an API Key

[Purchase a subscription](/subscription) first.

Then [go to the API Keys section](/api-keys) on the Dashboard to manage your account's API key.

## Option 1: HTTP Header

Create a request header for `Mailsac-Key`.

```bash
HTTP/1.x 200 OK
GET /api/addresses/test@example.com/messages
Host: mailsac.com
Mailsac-Key: eoj1mn7x5y61w0egs25j6xrvs6lwrrld0oh43rj583cgdps10tokp2ceux9s6ri8
```

## Option 2: Query String Parameter

In the query section of the URL (after `?`) add a parameter for `_mailsacKey`.

```bash
https://mailsac.com/api/addresses/test@example.com/messages?_mailsacKey=eoj1mn7x5y61w0egs25j6xrvs6lwrrld0oh43rj583cgdps10tokp2ceux9s6ri8
```

Field | Description
------|-------------
_mailsacKey | your API key


## Option 3: Request JSON Body

```json
{
    "_mailsacKey": "eoj1mn7x5y61w0egs25j6xrvs6lwrrld0oh43rj583cgdps10tokp2ceux9s6ri8"
}
```

During a POST or PUT request, add a JSON field for `_mailsacKey`.

Field | Description
------|-------------
_mailsacKey | your API key
