This method updates a message in a channel.


## Arguments

{ARGS}

## Valid message types

Only messages posted by the authenticated user are able to be updated using this method. This includes regular chat messages, as well as messages containing the `me_message` subtype.

Attempting to update other message types will return a `cant_update_message` error.

## Response

	{
		"ok": true,
		"channel": "C024BE91L",
		"ts": "1401383885.000061",
		"text": "Updated Text"
	}

The response includes the `text`, `channel` and `timestamp` properties of the
updated message so clients can keep their local copies of the message in sync.


## Errors

{ERRORS}
