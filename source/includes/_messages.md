# Email Messages API

## Permissions and Disposability

By default, all emails sent to Mailsac are accepted and public. They are recycled
regularly unless starred.

Buying and reserving an email address means only you can see messages sent to it.

Anyone can request messages on a public (non-owned) inbox. Anyone can also delete
messages from public inboxes.


## Example Email Message Object

```json
{
    "_id": "wLf7biMiadm6fPXHVE",
    "from": [Recipient],
    "to": [Recipient],
    "cc": [Recipient],
    "bcc": [Recipient],
    "subject": "hi bobby",
    "savedBy": "bob",
    "inbox": "test@example.com",
    "originalInbox": "test@example.com",
    "domain": "example.com",
    "received": "2016-08-16T02:59:13.406Z",
    "size": 6821,
    "attachments": ["def20078c72e1e72043e910734e5efbc"],
    "ip": "195.3.2.7",
    "via": "8.55.32.199",
    "folder": "inbox",
    "labels": ["Events"],
    "read": false,
    "rtls": true,
    "links": ["https://google.com"],
    "spam": 0.001019620767513069
}
```

Field | Description
------|--------------
\_id  | unique identifier of the email
from  | array of `Recipient` objects (see below)
to    | array of `Recipient` objects (see below)
cc    | array of `Recipient` objects (see below)
bcc   | array of `Recipient` objects (see below)
subject | email subject line
savedBy | when starred, this is the `Account._id`
inbox | email address to which this message belongs
originalInbox | email address which originally received the message. This will only be different if the message was forwarded, or rerouted to a catch-all inbox.
domain | hostname domain for the inbox
received | ISO 8601 date and time string
size | content length in bytes of the original raw email message
attachments | `null` or array of MD5 hashes of attachments. Use the [Attachments API](#attachments-api) to get a message's attachments.
ip | remote SMTP server that sent the mail to the server at `via`
via | IP address of SMTP server that received the message from `ip`
folder | inbox folder, one of: inbox, all, sent, spam, trash, drafts
labels | array of strings - custom inbox labels
read | read status - true=read, false=unread
rtls | received over TLS (encrypted)
links | list of any URLs that were found in the message contents
spam | result of a spam filter scan, between 0.0 and 1.0, where 1.0 indicates high likelihood of being spam


## Example Recipient Object

```json
{
    "name": "Bill Jones",
    "address": "billjones@example.com"
}
```

Field | Description
------|--------------
name  | friendly email name, optional part of the transport so may be empty string
address | email address


## List Inbox Email Messages
### `GET /api/addresses/:email/messages`

```json
[{
  "inbox": "test@example.com",
  "to": [{
    "address": "test@example.com",
    "name": ""
  }],
  "_id": "XOYj2t0MMe92286kf2hsOFNuPLxkjt3eQ",
  "from": [{
    "address": "tomriddle@example.com",
    "name": "Tom Riddle"
  }],
  "subject": "Page 2",
  "received": "2014-10-02T09:05:51.484Z",
  "size": 6210,
  "attachments": ["1e72043e910def20078c72e734e5efbc"]
}, {
  "inbox": "test@example.com",
  "to": [{
    "address": "test@example.com",
    "name": ""
  }],
  "savedBy": "bob",
  "_id": "vUSsCSHLC9vJkYBPruWhyhTZJXOaIIrm-0",
  "from": [{
    "address": "nounverb@example.com",
    "name": "Noun Verb"
  }],
  "subject": "Weeping",
  "received": "2016-10-10T16:58:59.131Z",
  "size": 115313,
  "attachments": null
}]
```

This is how to check the mail. Get a list of messages for the `:email` address.

The objects are abbreviated to provide basic metadata.


## List Starred/Saved Messages
### `GET /api/addresses/starred/messages`

Get a list of messages that have been saved and made private for the user.


## Get a Single Email Message
### `GET /api/addresses/:email/messages/:messageId`

```json
{
    "_id": "wLf7biMiadm6fPXHVE",
    "from": [Recipient],
    "to": [Recipient],
    "cc": [Recipient],
    "bcc": [Recipient],
    "subject": "hi bobby",
    "savedBy": "bob",
    "inbox": "test@example.com",
    "originalInbox": "test@example.com",
    "domain": "example.com",
    "received": "2016-08-16T02:59:13.406Z",
    "size": 6821,
    "attachments": ["def20078c72e1e72043e910734e5efbc"],
    "ip": "195.3.2.7",
    "via": "8.55.32.199",
    "folder": "inbox",
    "labels": ["Events"],
    "read": false,
    "rtls": true,
    "links": ["https://google.com"],
    "spam": 0.001019620767513069
}
```

Get an individual message.

### Path Params

Field | Description
------|------------
:email | email address
:messageId | the Mailsac `Message._id`


## List All Private Messages
### `GET /api/inbox`

```json
{
    "limit": 30,
    "skip": 0,
    "unread": 0,
    "since": "2014-09-29",
    "messages": [{
      "inbox": "test@example.com",
      "to": [{
        "address": "test@example.com",
        "name": ""
      }],
      "_id": "XOYj2t0MMe92286kf2hsOFNuPLxkjt3eQ",
      "from": [{
        "address": "tomriddle@example.com",
        "name": "Tom Riddle"
      }],
      "subject": "Page 2",
      "received": "2014-10-02T09:05:51.484Z",
      "size": 6210,
      "attachments": ["1e72043e910def20078c72e734e5efbc"]
    }, {
      "inbox": "test@example.com",
      "originalInbox": "test@example.com",
      "to": [{
        "address": "test@example.com",
        "name": ""
      }],
      "savedBy": "bob",
      "_id": "vUSsCSHLC9vJkYBPruWhyhTZJXOaIIrm-0",
      "from": [{
        "address": "nounverb@example.com",
        "name": "Noun Verb"
      }],
      "subject": "Weeping",
      "received": "2016-10-10T16:58:59.131Z",
      "size": 115313,
      "attachments": null,
      "spam": 0.001019620767513069
    }]
}
```

This is how to check the mail across all of an account's private email addresses.

### Query Params

Field | Description
------|------------
limit | integer - how many messages to return
skip | integer - how many messages to skip (like paging)
unread | integer - how many messages *in the response list* are unread
since | date / datetime - only fetch messages since this date or time


## Search Private Messages
### GET /api/inbox-search

```json
{
    "query": "",
    "messages": [{
      "inbox": "test@example.com",
      "to": [{
        "address": "test@example.com",
        "name": ""
      }],
      "_id": "XOYj2t0MMe92286kf2hsOFNuPLxkjt3eQ",
      "from": [{
        "address": "tomriddle@example.com",
        "name": "Tom Riddle"
      }],
      "subject": "Page 2",
      "received": "2014-10-02T09:05:51.484Z",
      "size": 6210,
      "attachments": ["1e72043e910def20078c72e734e5efbc"]
    }, {
      "inbox": "test@example.com",
      "to": [{
        "address": "test@example.com",
        "name": ""
      }],
      "savedBy": "bob",
      "_id": "vUSsCSHLC9vJkYBPruWhyhTZJXOaIIrm-0",
      "from": [{
        "address": "nounverb@example.com",
        "name": "Noun Verb"
      }],
      "subject": "Weeping",
      "received": "2016-10-10T16:58:59.131Z",
      "size": 115313,
      "attachments": null,
      "spam": 0.001019620767513069
    }]
}
```

Search for messages in private address. `query` matches the `to`, `from`,
and `subject`.


## Star/Save a Message
### `PUT /api/addresses/:email/messages/:messageId/star`

> Response body

```json
{
    "savedBy": "bob",
    "inbox": "test@example.com",
    "originalInbox": "test@example.com",
    "to": [{
        "address": "test@example.com",
        "name": ""
    }],
    "_id": "C9vJkYBIIrmPruWhyhTZJXOavUSsCSHL",
    "from": [{
        "address": "joe@example.com",
        "name": ""
    }],
    "subject": "Saved",
    "received": "2017-01-10T16:58:59.131Z",
    "size": 115313,
    "attachments": null
}
```

Toggle starred status so it will not be automatically removed.

There is no PUT body.

It returns only the message metadata.


## Add Label to a Message
### `PUT /api/addresses/:email/messages/:messageId/labels/:label`

> PUT Body

No body

> Response Body

```json
{
    "_id": "C9vJkYBIIrmPruWhyhTZJXOavUSsCSHL",
    "labels": ["Finance", "Taxes 2017"]
}
```

To help organize messages and group messages together, add a label
to a message. When successful, returns 200 with a subset of the message object.

When the label already exists on the message, the message is not modified
and the API endpoint returns 200.

## Remove a Label from a Message
### `DELETE /api/addresses/:email/messages/:messageId/labels/:label`

> DELETE Body

No body

> Response Body

```json
{
    "_id": "C9vJkYBIIrmPruWhyhTZJXOavUSsCSHL",
    "labels": ["Finance", "Taxes 2017"]
}
```

Removes a label from a message. Returns 200 with a subset of the message object
when successful.

When the label did not exists on the message, the message is not modified
and the API endpoint returns 200.


## Move Message to a Folder
### `PUT /api/addresses/:email/messages/:messageId/folder/:folder`

> Response Body

```json
{
    "_id": "C9vJkYBIIrmPruWhyhTZJXOavUSsCSHL",
    "folder": "all"
}
```

Move the message to a different mail folder. Valid folders are:

- `inbox` - default
- `all` - all archived mail
- `sent` - sent mail outbox
- `spam` - spam is unsolicited email, phishing or otherwise unwanted
- `trash` - mail which will soon be deleted
- `drafts` - messages which have not yet been sent

No PUT body is needed.

No other folders can be added. To organize mail, use labels.


## Set Message Read/Unread
### `PUT /api/addresses/:email/messages/:messageId/read/:readBoolean`


> Example Request

```bash
PUT /api/addresses/jimbo@mailsac.com/messages/C9vJkYBIIrmPruWhyhTZJXOavUSsCSHL/read/true
```

> Response Body

```json
{
    "_id": "C9vJkYBIIrmPruWhyhTZJXOavUSsCSHL",
    "read": true
}
```

Change the read state of a message. No PUT body is needed.

Pass `readBoolean` as `true` to mark the message as read, and `false` to mark it
as unread. The default is `false` - unread.


## Delete a Message
### `DELETE /api/addresses/:email/messages/:messageId`

Permanently removes a message. There is no history or trash bin.

## Get Message Headers
### `GET /api/headers/:email/:messageId`

```json
{
    "dkim-signature": "",
    "received": ["", ""],
    "x-facebook": "",
    "date": "",
    "to": "",
    "subject": "",
    "x-priority": "",
    "x-mailer": "",
    "return-path": "",
    "from": "",
    "reply-to": "",
    "errors-to": "",
    "x-facebook-notify": "",
    "list-unsubscribe": "",
    "x-facebook-priority": "",
    "x-auto-response-suppress": "",
    "require-recipient-valid-since": "",
    "message-id": "",
    "mime-version": "",
    "content-type": "",
    "received": ["", ""],
    "x-mailsac-whitelist": ""
}
```
Get the SMTP headers from an email message.

Use the querystring param `?download=1` to trigger file download in browser.

## Get Sanitized HTML
### `GET /api/body/:email/:messageId`

```bash
Content-Type: text/html
```

Get safe HTML from an email message. Scripts, images and links are stripped out.

Use the querystring param `?download=1` to trigger file download in browser.

## Get Message HTML
### `GET /api/dirty/:email/:messageId`

```bash
Content-Type: text/html
```

Get a message's HTML content.

Attached images are inlined and nothing has been stripped.

Use the querystring param `?download=1` to trigger file download in browser.

## Get Message Text
### `GET /api/text/:email/:messageId`

```bash
Content-Type: text/plain
```

Get a message's text content.

Use the querystring param `?download=1` to trigger file download in browser.


## Get Raw SMTP
### `GET /api/raw/:email/:messageId`

```bash
Content-Type: text/plain
```

The entire original SMTP message transport message.

Use the querystring param `?download=1` to trigger file download in browser.

## Send Email Messages
### `POST /api/outgoing-messages`

```json
{
    "to": "someone@example.com",
    "from": "somebody@mailsac.com",
    "subject": "Hey",
    "text": "Message text body [2561a.jpg]",
    "html": "<div>Message text body <img src=3D\"F3011FGE@hsd1.ca.comcast.net.\"></div>",
    "attachments": [{
        "cid": "F3011FGE@hsd1.ca.comcast.net.",
        "contentDisposition": "inline; filename=2561a.jpg",
        "content": "3asfji32gia...93as==",
        "encoding": "base64",
        "filename": "2561a.jpg",
    }],
    "received": ["by 130.7.72.46 with SMTP id g633m40907;\n Fri, 12 Oct 2016 17:26:53 -0700 (PDT)"],
    "raw": "raw SMTP message",
}
```

Send transactional text-only email messages.

Outgoing message credits must [be purchased](/pricing) first.

One credit will be used *per recipient* (as opposed to per email).

Either `text` or `html` is required to send a message.

When passing a `raw` SMTP package it should be a full SMTP message. All required
fields below must be included in the `raw` package.

### POST Body

Field | Description
------|------------
to | *required* - email addresses, currently capped at 3 - comma separated like `a@example.com,b@example.com` or as an array of recipient objects
from | *required* - the email address to be sent from - must be a verified domain the account is allowed to send from, formats could be a string email address or an array of recipient objects
subject | optional email subject line
text | non-HTML body
html | HTML body content
attachments | Array of attachment objects, see example code for format
received | Array of strings (when multiple) or string of previous received header(s)
raw | *overrides all other fields*; a raw SMTP message
