# User Account API

## Get Current User
### `GET /api/me`

> When the API key matches / user is authenticated

```json
{
    "_id": "my_username",
    "email": "outside-email@example.com",
    "messageLimit": 1000,
    "sendsRemaining": 362,
    "catchAll": 0,
    "privateAddressCredits": 1,
    "recents": [
        "example@mailsac.com",
        "mailsac@example.com"
    ],
    "apiAccess": 1,
}
```

Field | Description
------|-------------
\_id | account username
email | account email address (optional)
messageLimit | maximum allowed message history: starred messages + all messages on private addresses
sendsRemaining | number of outbound recipients left to be able to send a message to
catchAll | catch-all domain address, unused credits
privateAddressCredits | number of private addresses that the account is entitled to but has not yet reserved
recents | short list of recently viewed inboxes
apiAccess | indicates this is a paid API subscription account when > 0

> When API key is invalid / user is not authenticated

```json
null
```


## Get User Stats
### `GET /api/me/stats`

```json
{
    "storedMessages": 40,
    "starredMessages": 14,
    "addresses": [
        "example@mailsac.com",
        "mailsac@example.com"
    ],
    "nonOwnedInboxes": [
        "some-public-addr@mailsac.com"
    ],
    "inboxBytes": 15248
}
```

Get information about non-owned addresses with starred messages and
total starred messages, and list of owned addresses.

Field | Description
------|-------------
addresses | list of owned private addresses
nonOwnedInboxes | list of email addresses where user has starred messages but does not own the email address itself
starredMessages | total count of saved messages
storedMessages | total messages on all private addresses owned by the account
inboxBytes | sum size of all messages on private addresses


## Sign Out
### `POST /api/auth/logout`

Destroy the logged in user's session.

No `POST` body.

For cookie auth which works on the website only.
