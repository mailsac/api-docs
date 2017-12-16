# Web Socket API

You can receive email via web socket, for *private email addresses*.

To enable web socket forwarding, select "Edit" for the email address you
want to forward. Then check the checkbox for web socket forwarding,
and save. Web socket forwarding is not enabled by default.

<img src="images/forward-websocket-example.jpg" style="width: 330px; display: block" title="Enabling web socket forwarding for an email address" />

## Web Socket Examples

### Web Socket Test Page

<a href="https://sock.mailsac.com" target="_blank">https://sock.mailsac.com</a>

Receive emails in your web browser.

Experiment with the web socket gateway in realtime.

### Web Socket Node.js

Listen for Mailsac emails via websocket in this tiny Node.js example app.

<a href="https://github.com/ruffrey/mailsac-node-websocket-example" target="_blank">https://github.com/ruffrey/mailsac-node-websocket-example</a>

## Web Socket Connection Endpoint

```bash
# Example Web Socket Connection URL
wss://sock.mailsac.com/incoming-messages?_id=myusername;&amp;key=skkie9bksd2ad&amp;addresses=1@mailsac.com,2@mailsac.com
```

The web socket endpoint is `wss://sock.mailsac.com/incoming-messages`

### Query String Parameters

- `_id` - Your account username (aka _id).
- `key` - Your account's API key.
- `addresses` - A comma separated list of addresses you wish to listen for messages on.
These must be private addresses that your account owns.


## Web Socket Message Format

All web socket messages are JSON. After parsing the JSON, there will be a `status` field
with an HTTP status code (usually 200).

An email coming over the web socket will also have an `email` property, and its
value will be the same as the messages REST API, plus some additional fields:

## Example Web Socket Frame

```json
{
    "status": 200,
    "email": {
        "_id": "W0YIhkgWJxcZc1qujq2w6f1YPZwFc",
        "from": [
            {
                "name": "",
                "address": "foo@mailsac.com"
            }
        ],
        "to": [
            {
                "name": "",
                "address": "bar@mailsac.com"
            }
        ],
        "subject": "Hi hello",
        "originalInbox": "bar@mailsac.com",
        "inbox": "bar@mailsac.com",
        "domain": "mailsac.com",
        "received": "2017-05-01T01:39:27.940Z",
        "body": "<div>Yoooo</div>",
        "html": "<div>Yoooo</div>",
        "raw": "DKIM-Signature: v=1; a=rsa-sha256; c=relaxed/relaxed; d=mailsac.com;\r\n q=dns/txt;\n s=mailsacrelay;\r\n h=from:subject:to:mime-version:content-type:content-transfer-encoding:list-unsubscribe;\r\n b=redacted\r\nReceived: from localhost (127.0.0.1) by fortune with SMTP; Sun Apr 30\r\n 2017 21:40:10 GMT-0400 (EDT)\r\nContent-Type: text/plain\r\nFrom: foo@mailsac.com\r\nTo: bar@mailsac.com\r\nSubject: Hi me\r\nMessage-ID:\r\n \r\nList-Unsubscribe: \r\nContent-Transfer-Encoding: 7bit\r\nDate: Mon, 01 May 2017 01:40:10 +0000\r\nMIME-Version: 1.0\r\n\r\nYoooo",
        "headers": {
            "content-transfer-encoding": "7bit",
            "content-type": "text/plain",
            "date": "Mon, 01 May 2017 01:40:10 +0000",
            "dkim-signature": "&lt;redacted&gt;",
            "from": "foo@mailsac.com",
            "list-unsubscribe": "",
            "message-id": "",
            "mime-version": "1.0",
            "received": "from localhost (127.0.0.1) by fortune with SMTP; Sun Apr 30 2017 21:40:10 GMT-0400 (EDT)",
            "subject": "Hi me",
            "to": "bar@mailsac.com"
        },
        "text": "Yoooo"
    }
}
```
