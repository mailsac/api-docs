# Email Stats API

These APIs can be helpful in detecting spam, phishing, or other nefarious SMTP
activity.

Mailsac receives email for **tens of thousands** of domains, acting as a
honeypot.

Private addresses data is not returned by the this set of APIs.


## List Top Email Addresses
### `GET /api/mailstats/top-addresses`

> Example Request

```bash
GET /api/mailstats/top-addresses?startDate=2017-04-26T03:14:40.412Z&endDate=2017-04-28T07:14:40.412Z
```

```json
[
    {
        "_id": "apad@mailsac.com",
        "n": 10
    },
    {
        "_id": "foooo@mailsac.com",
        "n": 7
    },
    {
        "_id": "fcca3a@mailsac.com",
        "n": 6
    },
    {
        "_id": "11111@mailsac.com",
        "n": 4
    }
]
```

Search for the top **non-private addresses** that have been receiving mail.

### Request

Querystring Param | Format | Description
------|--------|-----------
startDate | date (UTC) | Required - Limit results to inboxes that received messages after this date.
endDate | date (UTC) | Required - Limit results to inboxes that received messages before this date.
limit | integer | Optional - Limit results to this many, default `20`, max `1000`.
skip | integer | Optional - Skip this many, default `0`.


### Response

Field | Description
------|--------|-----------
\_id | email address
n | count of messages in the inbox


## Blacklisted Domains and IPs
### `GET /api/mailstats/blacklist`

```json
[
    "example.com",
    "192.168.0.1"
]
```

Get an array of domains and IP addresses that have been blacklisted for
violating the terms of service and/or degrading the service experience for
other customers.


## Check Blacklist
### `GET /api/mailstats/blacklist/:domainOrIP`

Check if a domain or IP is on the blacklist.

Returns `404` if not blacklisted.

Returns `200` if blacklisted.
