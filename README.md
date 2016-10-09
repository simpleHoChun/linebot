# linebot

  [![NPM Version][npm-image]][npm-url]
  [![NPM Dependencies][dependencies-image]][dependencies-url]
  [![NPM Downloads][downloads-image]][downloads-url]

LINE Messaging API for Node.js

# About LINE Messaging API

Please refer to the official API documents for details.

https://devdocs.line.me

# Installation

```bash
$ npm install linebot --save
```

# Usage

```js
var linebot = require('linebot');

var bot = linebot({
	channelId: [CHANNEL_ID],
	channelSecret: [CHANNEL_SECRET],
	channelAccessToken: [CHANNEL_ACCESS_TOKEN]
});

bot.on('message', function (event) {
	event.reply(event.message.text).then(function (data) {
		// success
	}).catch(function(error) {
		// error
	});
});

bot.listen('/linewebhook', 3000);
```

# API

## linebot(config)
Create LineBot instance with specified configuration.
```js
var bot = linebot({
    channelId: [CHANNEL_ID],
    channelSecret: [CHANNEL_SECRET],
    channelAccessToken: [CHANNEL_ACCESS_TOKEN],
	verify: true // Verify `X-Line-Signature` header.
});
```

## LineBot.listen(webHookPath, port, callback)

Start [Express.js][express-url] web server on the specified `port`,
and accept POST request callback on the specified `webHookPath`.

This method is provided for convenience.
You can write you own server and use `verify` and `parse` methods to process webhook events.
See `examples/echo-express.js` for example.

## LineBot.verify(rawBody, signature)

Verify `X-Line-Signature` header

## LineBot.parse(body)

Process incoming webhook request, and raise an event.

## LineBot.on(eventType, eventHandler)

Raised when a [Webhook event][webhook-event-url] is received.
```js
bot.on('message',  function (event) { });
bot.on('follow',   function (event) { });
bot.on('unfollow', function (event) { });
bot.on('join',     function (event) { });
bot.on('leave',    function (event) { });
bot.on('postback', function (event) { });
```

## Event.reply(message)

Respond to the event.
`message` can be a string, a [Send message][send-message-url] object, or an array of [Send message][send-message-url] objects.

Return a [Promise][promise-url] object from [`node-fetch`][node-fetch-url] module.

This is a shorthand for `LineBot.reply(event.replyToken, message);`

```js
event.reply('Hello, world');

event.reply({ type: 'text', text: 'Hello, world' });

event.reply([
	{ type: 'text', text: 'Hello, world 1' },
	{ type: 'text', text: 'Hello, world 2' }
]);

event.reply({
	type: 'image',
	originalContentUrl: 'https://example.com/original.jpg',
	previewImageUrl: 'https://example.com/preview.jpg'
});

event.reply({
	type: 'video',
	originalContentUrl: 'https://example.com/original.mp4',
	previewImageUrl: 'https://example.com/preview.jpg'
});

event.reply({
	type: 'audio',
	originalContentUrl: 'https://example.com/original.m4a',
	duration: 240000
});

event.reply({
	type: 'location',
    title: 'my location',
    address: '〒150-0002 東京都渋谷区渋谷２丁目２１−１',
    latitude: 35.65910807942215,
    longitude: 139.70372892916203
});

event.reply({
	type: 'sticker',
	packageId: '1',
	stickerId: '1'
});
```

## Event.source.profile()

Get user profile information of the sender.

This is a shorthand for `LineBot.getUserProfile(event.source.userId);`

```js
event.source.profile().then(function (profile) {
	event.reply('Hello ' + profile.displayName);
});
```
## Event.message.content()

Get image, video, and audio data sent by users.

Return a Buffer object of binary data.

This is a shorthand for `LineBot.getMessageContent(event.message.messageId);`

```js
event.message.content().then(function (content) {
	console.log(content.toString('base64'));
});
```

# License

  [MIT](LICENSE)

[express-url]: http://expressjs.com
[webhook-event-url]: https://devdocs.line.me/en/#webhook-event-object
[send-message-url]: https://devdocs.line.me/en/#send-message-object
[promise-url]: https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise
[node-fetch-url]: https://github.com/bitinn/node-fetch
[npm-image]: https://img.shields.io/npm/v/linebot.svg
[npm-url]: https://npmjs.org/package/linebot
[dependencies-image]: https://david-dm.org/boybundit/linebot.svg
[dependencies-url]: https://david-dm.org/boybundit/linebot
[downloads-image]: https://img.shields.io/npm/dm/linebot.svg
[downloads-url]: https://npmjs.org/package/linebot
