# Free website indexability monitor

[![Monitor production indexability](https://github.com/unitedideas/website-indexability-monitor/actions/workflows/monitor.yml/badge.svg)](https://github.com/unitedideas/website-indexability-monitor/actions/workflows/monitor.yml)

Catch an accidental homepage `noindex` in a scheduled GitHub Action. The same run reports HTTP status, robots meta, `X-Robots-Tag`, canonical URL, robots.txt policy for eight named AI crawler tokens, and optional synthetic crawler responses.

No account, API key, package install, or hosted agent is required.

## Set it up

1. Select **Use this template** and create a repository.
2. In the new repository, open **Settings → Secrets and variables → Actions → Variables**.
3. Add a repository variable named `MONITORED_WEBSITE` with a public hostname or URL, such as `example.com`.
4. Open **Actions → Monitor production indexability → Run workflow** once to verify the target.

The workflow then runs every six hours. It fails when the returned homepage contains `noindex` in robots meta or `X-Robots-Tag`, so GitHub can surface the failure through your normal Actions notifications.

## What the check reports

- homepage HTTP response
- robots meta directives
- `X-Robots-Tag` directives
- canonical URL
- robots.txt policy for `OAI-SearchBot`, `GPTBot`, `OAI-AdsBot`, `Claude-SearchBot`, `ClaudeBot`, `Claude-User`, `PerplexityBot`, and `Google-Extended`
- separately labeled synthetic homepage responses for three AI search-crawler user agents

The workflow is intentionally conservative: a blocked AI crawler token, canonical mismatch, or restricted synthetic response is reported but does not fail the run by default. Edit the workflow inputs if those states should block your deployment.

## Evidence boundary

The check reads the public response, raw HTML, and headers without running browser JavaScript. Synthetic requests do not authenticate provider IP ranges. A clean run does not prove crawling, indexing, citation, ranking, or traffic.

GitHub schedules can be delayed during busy periods and may be disabled after prolonged repository inactivity. This template checks one homepage and relies on GitHub notifications; it is not a dashboard, real-crawler log, or dedicated alerting service.

## Need a managed check?

[Crawler Watch](https://actablesite.com/crawler-watch?utm_source=github&utm_medium=template&utm_campaign=crawler-watch) checks one public site every 15 minutes, confirms a changed state twice, and emails a dedicated alert. It also watches `sitemap.xml` and `llms.txt`. The plan is $9 per month.

## Source

The workflow uses the MIT-licensed [`unitedideas/actablesite-check`](https://github.com/unitedideas/actablesite-check) action pinned to its stable `v1` release line.

## License

MIT
