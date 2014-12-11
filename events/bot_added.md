# bot_changed event

	{
		"type": "bot_added",
		"bot": {
			"id": "B024BE7LH",
			"name": "hugbot",
			"icons": {
				"image_48": "https:\/\/slack.com\/path\/to\/hugbot_48.png"
			}
		}
	}

The `bot_added` event is sent to all connections for a team when an
integration "bot" is added. Clients can use this to update their local list
of bots.
