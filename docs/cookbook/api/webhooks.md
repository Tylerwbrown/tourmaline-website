---
id: webhooks
title: Webhooks
---

### Intro

There are two ways a bot can be run: polling, which is when the bot sends constant requests to Telegram and looks for new updates, and webhooks, which is where the bot listens for updates, and Telegram sends them as they come in. Obviously polling has some drawbacks, the main ones being the bandwidth and processing power required to be constantly sending out HTTP requests. It is advised to never use polling in production, and this is why we have webhooks.

> **Note:** Telegram requires webhooks to be run using HTTPS.

### Setting a Webhook

Tourmaline comes with a few webhook specific methods: `set_webhook`, `unset_webhook`, `get_webhook_info`, `delete_webhook`, and `serve`. To set a webhook and start your bot listening for requests it's pretty simple.

```crystal
bot.set_webhook("https://example.com/webhook/path")
bot.serve("0.0.0.0", 3400)
```

It's recommended by telegram to include your bot's API key in the webhook path, since then only Telegram will know exactly where to send requests.

### Unsetting or Deleting a Webhook

Should you decide to switch back to polling it would be good to either unset or delete your webhook so that Telegram stops sending updates there. Doing that is easy:

```crystal
bot.unset_webhook # will set the webhook to ""
bot.delete_webhok # will delete the webhook outright
```

### Local Webhook Testing

It can be nice to test your bot locally using webhooks, as they are a bit faster than polling. Doing so is pretty easy if you use Tourmaline in conjunction with the [ngrok.cr](https://github.com/watzon/ngrok.cr) shard. Here is a simple example of that working:

```crystal
Ngrok.start({ addr: "127.0.0.1:3400" }) do |ngrok|
  bot.set_webhook(ngrok.ngrok_url_https)
  bot.serve("127.0.0.1", 3400)
end
```