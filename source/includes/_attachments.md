# Attachments API

## List Attachments on a Message
### `GET /api/addresses/:email/messages/:messageId/attachments`

```json
[{
    "checksum": "0af24bbe413e908d32d3fbd0fe26094f",
    "contentDisposition": "attachment",
    "contentId": "d1518c409f9cfa45d2aaaab1514fe183",
    "contentType": "application/pdf",
    "fileName": "ienbweg.pdf",
    "length": 65979,
    "transferEncoding": "base64"
}]
```

List the metadata for all attachments on a message.

## Download an Attachment
### `GET /api/addresses/:email/messages/:messageId/attachments/:attachmentIdentifier`

Download an attachment on a message.

The `Content-Type` will be set to the `contentType` field from the attachment
metadata.


## List Common Attachments
### `GET /api/mailstats/common-attachments`

> Example Request

```bash
GET /api/mailstats/common-attachments?startDate=2017-03-25&endDate=2017-03-28T02:53:50.269Z&limit=100&skip=1000
```

```json
[
    {
        "_id": "d98f50c1cb9598b1d3cdf69779d5a435",
        "n": 3
    },
    {
        "_id": "b556203c4d5f74def67e1b7087548907",
        "n": 3
    },
    {
        "_id": "a58180dd3c36128f48e425f8f15ce204",
        "n": 2
    },
    {
        "_id": "29f28a35b7fe4c870a5db3b0b3fe712c",
        "n": 2
    },
    {
        "_id": "f7f3f975bdfb0044a96c1dc27d18d0fb",
        "n": 2
    }
]
```

Search for attachments that were received during the requested time period.

Limited to non-private inboxes.

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
\_id | MD5 hash of the attachment file
n | count of messages with this attachment



## Count Attachments by MD5
### `GET /api/mailstats/common-attachments/:md5sum/count`

```json
{
    "n": 70
}
```

Count the number of email messages that have attachments with this MD5 sum.

Limited to non-private inboxes.

## Get Messages with Attachment
### `GET /api/mailstats/common-attachments/:md5sum`

```json
[
    {
        "inbox": "xh@mailsac.com",
        "_id": "qC4068KcUG0ht6eBfnFcEtXs7",
        "from": [
            {
                "address": "a@b.com",
                "name": "Aaa"
            }
        ],
        "subject": "Stuff",
        "received": "2017-04-23T05:49:29.297Z",
        "attachments": [
            "06f33e07967f624c57445666b9efb1a9",
            "666b9ef33066967f24c57445e07fb1a0"
        ]
    },
    {
        "inbox": "joe@example.com",
        "_id": "Ms8PPC9FKPOwi2o8OwMs89CQw",
        "from": [
            {
                "address": "belmo@a.co",
            }
        ],
        "subject": "Chicken sandwich",
        "received": "2017-04-26T05:42:29.444Z",
        "attachments": [
            "06f33e07967f624c57445666b9efb1a9"
        ]
    }
]
```

List the email messages that have attachments with the requested MD5 sum.

Limited to non-private inboxes.


## Download a Common Attachment
### `GET /api/mailstats/common-attachments/:md5sum/download`

Download an attachment with the MD5 sum requested.

<aside class="warning">Warning: attachments may contain viruses!</aside>
