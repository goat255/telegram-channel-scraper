# AGENTS.md: Telegram Channel Scraper

Guide for AI agents (Claude, Codex, any MCP or HTTP client) that need Telegram channel messages.

## When to use it

Use this actor when a task needs the recent or full message history of one or more Telegram channels: daily briefings, news tracking, channel archives, alerting or datasets for summaries. It can also return a summary row per channel and the channel's visible discussion members. It does not post or log in.

## Call it

**MCP (recommended):** add the Apify MCP server with this actor enabled:

```json
{
  "mcpServers": {
    "apify": {
      "url": "https://mcp.apify.com/?actors=goat255/telegram-channel-scraper",
      "headers": { "Authorization": "Bearer YOUR_APIFY_TOKEN" }
    }
  }
}
```

**HTTP:** one synchronous call returns the rows:

```
POST https://api.apify.com/v2/acts/goat255~telegram-channel-scraper/run-sync-get-dataset-items?token=YOUR_APIFY_TOKEN
Content-Type: application/json

{"channels": ["example_channel"], "maxMessagesPerChannel": 50}
```

## Input

| Field | Type | Default | Meaning |
|---|---|---|---|
| `channels` | array of strings | required | Handles or links: `example_channel`, `@example_channel`, `https://t.me/example_channel` |
| `maxMessagesPerChannel` | integer | `50` | Messages per channel, newest first |
| `includeChannelInfo` | boolean | `true` | Add one summary row per channel |
| `exportMembers` | boolean | `false` | Also export the channel's visible discussion members |
| `maxMembersPerChannel` | integer | `200` | Cap on members per channel when `exportMembers` is on |
| `concurrency` | integer | `4` | Channels processed in parallel |
| `delayBetweenRequests` | number | `1.0` | Seconds between page fetches inside one channel |
| `proxyConfiguration` | object | off | Optional proxy for large jobs |

## Output

**Message rows:**

| Field | Meaning |
|---|---|
| `channel`, `messageId`, `url` | Channel handle, message id, direct link |
| `datetime` | ISO 8601 with timezone |
| `views` | View count as a number |
| `text` | Cleaned text, line breaks kept |
| `hasPhoto`, `hasVideo`, `hasDocument`, `hasVoice` | Media flags |
| `photoUrls`, `videoUrls` | Media URLs |
| `forwardedFrom`, `forwardedFromUrl` | Origin of a forwarded message |
| `links` | Outbound links in the text |

**Channel summary row** (`type: "channel_info"`): `title`, `subscribers`, `description`, `photoUrl`, `photoCount`, `videoCount`, `fileCount`, `linkCount`.

**Member rows** (`type: "member"`): `channel`, `username`, `profileUrl`, `displayName`, `avatarUrl`. A channel without public discussion returns one row with `type: "member_export"` and `result: "not_available"`.

## Tips for agents

- Batch many channels into one run.
- Split rows by `type`: message rows have no `type`, summary rows are `channel_info`, member rows are `member`.
- For a daily briefing, schedule the run and keep only messages whose `datetime` falls in the last 24 hours.
- For members only, set `maxMessagesPerChannel` to `1`.

Store page: https://apify.com/goat255/telegram-channel-scraper
