# Telegram Release Notify

GitHub Action for sending release/deploy notifications to Telegram.

## Usage

```yaml
- uses: enyonee/telegram-notify-action@v1
  with:
    bot_token: ${{ secrets.TELEGRAM_BOT_TOKEN }}
    chat_id: ${{ secrets.TELEGRAM_CHAT_ID }}
```

### Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `bot_token` | yes | | Telegram bot token |
| `chat_id` | yes | | Telegram chat ID |
| `status` | no | `success` | `success` or `failure` |
| `message` | no | | Custom message (overrides default) |

### Full example

```yaml
- uses: enyonee/telegram-notify-action@v1
  if: always()
  with:
    bot_token: ${{ secrets.TELEGRAM_BOT_TOKEN }}
    chat_id: ${{ secrets.TELEGRAM_CHAT_ID }}
    status: ${{ job.status }}
```
