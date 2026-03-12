# Telegram Notify Action

Universal Telegram notifications for GitHub Actions CI/CD.

## Template format

```
{icon} {project} {event}
📌 {branch} by {actor}

{body}

🔗 View logs
```

Icon (🟢/🔴) is auto-selected from `status`. Link is auto-generated from github context.

## Usage

### CI notification

```yaml
- uses: enyonee/telegram-notify-action@v2
  with:
    bot_token: ${{ secrets.TELEGRAM_BOT_TOKEN }}
    chat_id: ${{ secrets.TELEGRAM_CHAT_ID }}
    status: ${{ job.status }}
    project: MyApp
    event: CI
    body: |
      🔍 Lint: ${{ needs.lint.result == 'success' && '✅' || '❌' }}
      🧪 Tests: ${{ needs.test.result == 'success' && '✅' || '❌' }}
```

### Deploy notification

```yaml
- uses: enyonee/telegram-notify-action@v2
  with:
    bot_token: ${{ secrets.TELEGRAM_BOT_TOKEN }}
    chat_id: ${{ secrets.TELEGRAM_CHAT_ID }}
    status: ${{ job.status }}
    project: MyApp
    event: deploy
    changelog: ${{ steps.changelog.outputs.log }}
```

### Full custom message (backward compat)

```yaml
- uses: enyonee/telegram-notify-action@v2
  with:
    bot_token: ${{ secrets.TELEGRAM_BOT_TOKEN }}
    chat_id: ${{ secrets.TELEGRAM_CHAT_ID }}
    message: "Custom HTML message here"
```

## Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `bot_token` | yes | | Telegram bot token |
| `chat_id` | yes | | Telegram chat ID |
| `status` | no | `success` | `success` or `failure` (sets icon) |
| `project` | no | repo name | Project name for header |
| `event` | no | | Event type: CI, deploy, K8s tests, etc. |
| `branch` | no | `github.ref_name` | Branch or tag name |
| `body` | no | | Main content (HTML) |
| `changelog` | no | | Changelog for deploy notifications |
| `message` | no | | Full custom message (overrides template) |
