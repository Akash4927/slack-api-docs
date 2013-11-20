# Message Formatting

All text in Slack uses the same system of escaping: chat messages, direct messages, file comments, etc.


## URLs and Escaping

Messages can contain any displayable Unicode sequence of characters (all messages must use UTF-8 encoding), 
but need to be slightly escaped by clients. Before sending messages to the server, clients should make the
following transforms:

    & replaced with &amp;
    < replaced with &lt;
    > replaced with &gt;

For example:

    User types     : Hello & <world>
    Sent to server : Hello &amp; &lt;world&gt;

This is done so that messages can contain special escaped sequences. For example, to refer to a channel or
user within a message, a client should send the following messages:

    Why not join <#C024BE7LR|general>?

    Hey <@U024BE7LH|bob>, did you see my file?

These escape sequences are then forwarded to all members of the channel as usual, and clients can format
these links specially. The readable name can be included after the ID, by separating them with a pipe (`|`) 
character. Both of these formats are valid:

    <@U024BE7LH>
    <@U024BE7LH|bob>

For regular URL links, clients should just include the URL in the message; It is not the responsibility of 
individual clients to detect URLs within typed messages. For example, the following messages can be sent 
to the server:

    This message contains a URL http://foo.com/

    So does this one: www.foo.com

URL detection is performed by the server. We do this so that URL detection (a non-trivial operation) is
performed consistently across multiple clients. The example messages will be broadcast to other clients 
as follows:

    This message contains a URL <http://foo.com/>

    So does this one: <http://www.foo.com|www.foo.com>

In the first case, the URL is detected as-is. In the second, the full URL is included first, then a pipe 
character (`|`), then finally the originally typed version of the URL.

There is a further special token format which is used to denote various things:

    <!foo>

Where `foo` is a special command. Currently defined commands are `!channel`, `!group` and `!everyone`
(group is just a synonym for channel - both can be used in channels and groups).
Commands that are not recognised can be output as-is, without the enclosing brackets.

To display messages in a web-client, the client should take the following steps:

1.  Detect all sequences matching `<(.*?)>`
2.  Within those sequences, format content starting with `#C` as a channel link
3.  Within those sequences, format content starting with `@U` as a user link
4.  Within those sequences, format content starting with `!` according to the rules for the special command.
5.  For remaining sequences, format as a link
6.  Once the format has been determined, check for a pipe - if present, use the text following the pipe as the link label

Because the ampersands and angled brackets are already escaped, no further translation need take place 
(for a web-client). The server ensures that no extra un-escaped angled brackets or ampersands are 
included in the message.

When a client sends a message, the response that is returned by the server contains the server-formatted
version of the message. Clients should use this to replace their own local version of the message so that
urls are correctly highlighted.


## API URLs

URLs that have a protocol of `api::` must be handled separately from normal URLs. The intention is that clicks
on these URLs perform an API call instead of redirecting to a web page. The format of these URLs is:

    api::method?param1=value1&param2=value2

An example url might be:

    <api::services.mute?service_id=7|Make it stop>

For the above example, the api call
would look like this:

    services.mute - method
    token=xxxxx
    service_id=7

A client MIGHT want to confirm with the user before making these calls, but either way, it will perform an api call
as normal with the specified parameters and using the user's token.


## Emoji

Slack attempts to normalize emoji across multiple platforms using the follow approach:

1. All emoji sent to Slack (as chat message to the message server, or arguments to the API) are translated into
   the common 'colon' format (e.g. `:smile:`)
2. At display time, Slack clients are encouraged to convert these colon-sequences into native Emoji where available,
   otherwise fallback to images.

The Slack message server and API handle conversion from several binary emoji formats - the Unicode Unified format
(used by OSX 10.7+ and iOS 6+), the Softbank format (used by iOS 5) and the Google format (used by some Android
devices). These Unicode code points will be converted into their colon-format equivalents.

The list of emoji supported are taken from https://github.com/iamcal/emoji-data
