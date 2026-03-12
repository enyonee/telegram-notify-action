# Telegram Notify Action

Universal Telegram notifications for GitHub Actions CI/CD.

## Template format

```
{icon} {project} {version} {event}
📌 {branch} by {actor}
🔗 #{issue} · PR #{pr}

{body}

→ View logs
```

All fields optional. Icon (🟢/🔴) auto-selected from `status`. Links auto-generated from github context.

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
    pr: ${{ github.event.pull_request.number }}
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
    version: ${{ github.ref_name }}
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
| `version` | no | | App version (e.g. v2.18.0) |
| `branch` | no | `github.ref_name` | Branch or tag name |
| `issue` | no | | Issue number, auto-links to GitHub |
| `pr` | no | | PR number, auto-links to GitHub |
| `body` | no | | Main content (HTML) |
| `changelog` | no | | Changelog for deploy notifications |
| `message` | no | | Full custom message (overrides template) |
