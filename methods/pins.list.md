
This method lists the items pinned to a channel.

## Arguments

{ARGS}


## Response

The response contains a list of pinned items in a channel.

	{
		"ok": true,
		"items": [
			{
				"type": "message",
				"channel": "C2147483705",
				"message": {...},
				"created": 1456335673
			},
			{
				"type": "file",
				"file": { ... },
				"created": 1456335673
			}
			{
				"type": "file_comment",
				"file": { ... },
				"comment": { ... },
				"created": 1456335673
			}
		]
	}

Different item types can be pinned. Every item in the list has a `type` property, and the other properties depend on the type of item. The possible types are:

 * **`message`**: the item will have a `message` property containing a [message object](/docs/messages) and a `channel` property containing the channel ID for the message.
 * **`file`**: this item will have a `file` property containing a [file object](/types/file).
 * **`file_comment`**: the item will have a `file` property containing the [file object](/types/file) and a `comment` property containing the file comment.

The `created` property on each item is a unix timestamp representing when the item was pinned.

## Errors

{ERRORS}
## Warnings

{WARNINGS}
