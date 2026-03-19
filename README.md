name: Daily AI News
on:
  schedule:
    - cron: '0 0 * * *'  # 每天 UTC 0点（即北京时间 8点）
  workflow_dispatch:

jobs:
  fetch-and-send:
    runs-on: ubuntu-latest
    steps:
      - name: Fetch AI news
        run: |
          curl -s "https://api.github.com/search/issues?q=agent+ai+created:$(date +%Y-%m-%d)" \
            -H "Authorization: token ${{ secrets.GITHUB_TOKEN }}" > news.json
      - name: Send to OpenClaw
        run: |
          curl -X POST "${{ secrets.OPENCLAW_WEBHOOK }}" \
            -H "Content-Type: application/json" \
            -d @news.json
