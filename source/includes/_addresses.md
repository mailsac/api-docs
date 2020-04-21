# Email Addresses API

## Example Email Address Object

```json
{
    "_id": "asdf@example.com",
    "created": "2015-04-05T15:10:33.234Z",
    "enablews": true,
    "forward": "bill@example.com",
    "webhook": "https://example.com/email-callback",
    "owner": "jimbo",
    "encryptedInbox": "inbox-efeaefa6e78d9abba34c4"
}
```

Field | Description
------|------------
\_id         | the unique identifier of this email address is the email address itself
created     | ISO 8601 date and time string
enablews    | boolean defaulting `false` indicating whether to publish messages via web socket when the user owning this inbox is subscribed
forward     | email address where messages to this inbox will be forwarded
webhook     | URL where messages to this inbox will be forwarded
owner       | `Account._id` that owns the address (owns the privacy)
encryptedInbox | an alternate inbox prefix that will result in the message being delivered to this inbox `_id`

## List All Private Addresses
### `GET /api/addresses`

```json
[{
    "_id": "asdf@mailsac.com",
    "created": "2013-02-05T15:10:33.234Z",
    "enablews": true,
    "forward": "asdf@example.com",
    "webhook": "https://example.com/email-callback",
    "owner": "your account._id",
    "encryptedInbox": "inbox-d6da59f7a6e78d9abba34c4"
}, {
    "_id": "somewhere@mailsac.com",
    "created": "2013-02-05T15:10:33.234Z",
    "enablews": false,
    "forward": null,
    "webhook": null,
    "owner": "your account._id",
    "encryptedInbox": "inbox-a6e7a59f7ba34c48d9abd6d"
}]
```

Get an array of private inbox address objects for the account.


## Get a Specific Private Address
### `GET /api/addresses/:email`

```json
{
    "_id": "somewhere@mailsac.com",
    "created": "2013-02-05T15:10:33.234Z",
    "enablews": true,
    "forward": "somewhere@example.com",
    "webhook": "https://example.com/email-callback",
    "owner": "your account._id",
    "encryptedInbox": "inbox-d6da59f7a6e78d9abba34c4"
}
```

Get a single address object.

Returns an object if owned by the user or not owned.

Returns 401 if owned by other user.


## Check Address Ownership
### `GET /api/addresses/:email/availability`

```json
{
    "available": true,
    "email": "ae638ef@mailsac.com",
    "owned": false
}
```

Check if an address is private (AKA owned).



## Reserve a Private Email Address
### `POST /api/addresses/:email`

```json
{
    "_id": "somewhere@mailsac.com",
    "created": "2013-02-05T15:10:33.234Z",
    "enablews": true,
    "forward": "somewhere@example.com",
    "webhook": "https://example.com/email-callback",
    "owner": "your account._id",
    "encryptedInbox": "inbox-d6da59f7a6e78d9abba34c4"
}
```

Reserve ownership of a private email address.

No POST body is required.

Returns 200 if successfully reserves the address.

Returns 401 if owned by other user.

Returns 400 if it is already owned by the user.


## Release a Private Address
### `DELETE /api/addresses/:email`

Release ownership of a private address.

Returns 200 if successfully releases the address.

Returns 401 if owned by other user.

Returns 400 if it is not owned.

### Query Params

Field | Description
------|------------
deleteAddressMessages| true - Delete all messages associated with private address

## Forward an Email Address
### `PUT /api/private-address-forwarding/:email`

```json
{
    "forward": "newemail@example.com",
    "enablews": true,
    "webhook": "https://example.com/email-callback"
}
```

For a privately owned address `:email`, set it to forward to another email.

### PUT Body

Field | Description
------|-------------
forward | email address - SMTP forwarding / standard email forwarding - set to `""` or `null` to disable forwarding
enablews | boolean, defaults `false` - set to `true` to enable web socket forwarding (see Web Socket API)
webhook | url - set to your public webhook endpoint to receive mail via webhook - set to `""` or `null` to disable webhooks
