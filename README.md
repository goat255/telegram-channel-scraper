# Telegram Channel Scraper

Wake up to everything your Telegram channels posted overnight. Export messages from any list of Telegram channels with text, date, views, media, forwards and links in one clean row each, plus a channel summary and the channel's visible members. No login, no API key.

[![Watch the demo on YouTube](https://img.youtube.com/vi/FgSN9dohFhM/maxresdefault.jpg)](https://www.youtube.com/watch?v=FgSN9dohFhM)

**[Run it on Apify →](https://apify.com/goat255/telegram-channel-scraper)**

## What you get

- **One row per message**, newest first: `channel`, `messageId`, `url`, `datetime`, `views` and the cleaned `text`.
- **Media**: `hasPhoto`, `hasVideo`, `hasDocument`, `hasVoice` flags plus `photoUrls` and `videoUrls`.
- **Context**: `forwardedFrom`, `forwardedFromUrl` and every outbound link in `links`.
- **A channel summary row** (`type: "channel_info"`): `title`, `subscribers`, `description`, `photoUrl` and media counts.
- **Visible members** (optional): the people taking part in the channel's discussion, with `username`, `displayName`, `profileUrl` and `avatarUrl`.

```json
{
  "channel": "example_channel",
  "messageId": 1234,
  "url": "https://t.me/example_channel/1234",
  "datetime": "2026-02-10T17:43:45+00:00",
  "views": 1740000,
  "text": "Big news today. Read the full post here.",
  "hasPhoto": false,
  "hasVideo": true,
  "videoUrls": ["https://example.com/video.mp4"],
  "forwardedFrom": null,
  "links": ["https://example.com/full-post"]
}
```

## What people use it for

- Morning briefings: schedule a nightly run and read one sheet of everything your channels posted.
- News and announcement tracking across many channels at once.
- Channel archives with dates and view counts for reporting.
- Datasets of posts for analysis, alerting or AI summaries.

## Run it

From the Apify Console, add channel handles or links and press Start. Or call it from code:

```bash
curl -X POST "https://api.apify.com/v2/acts/goat255~telegram-channel-scraper/run-sync-get-dataset-items?token=YOUR_APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"channels": ["example_channel", "https://t.me/another_example"], "maxMessagesPerChannel": 100}'
```

## Use it from an AI agent

Works with Claude, Codex and any agent that speaks MCP or HTTP. See [AGENTS.md](AGENTS.md) for the input, output and a ready-made MCP setup.

## Pricing

Pay per message returned. Current prices are on the [Apify Store page](https://apify.com/goat255/telegram-channel-scraper).

## More actors

This page is a showcase; the actor itself runs on Apify. **[Check our profile for more such actors →](https://apify.com/goat255)**
