# Website Indexability Gate

[![Monitor production indexability](https://github.com/unitedideas/website-indexability-monitor/actions/workflows/monitor.yml/badge.svg)](https://github.com/unitedideas/website-indexability-monitor/actions/workflows/monitor.yml)

Fail a GitHub Actions job when a production homepage accidentally returns `noindex`. The same run reports HTTP status, robots meta, `X-Robots-Tag`, canonical URL, robots.txt policy, and optional synthetic AI crawler responses.

One public URL is the only required input. No account, API key, package install, or hosted agent is required.

```yaml
- name: Guard production indexability
  uses: unitedideas/website-indexability-monitor@v1
  with:
    website: example.com
```

The default fails only when robots meta or `X-Robots-Tag` contains `noindex`. Status, canonical, robots.txt, and optional edge evidence stay visible in the job summary without turning every warning into a broken deploy.

## Schedule the same check

Select **Use this template** to create a repository with a six-hour schedule, then add a repository variable named `MONITORED_WEBSITE`. Run **Actions → Monitor production indexability → Run workflow** once to verify the target.

The template relies on GitHub's normal Actions notifications. You can change its schedule or failure inputs in `.github/workflows/monitor.yml`.

## Inputs

| Input | Default | Effect |
| --- | --- | --- |
| `website` | required | Public hostname or URL to inspect. |
| `fail-on-noindex` | `true` | Fails when homepage robots meta or `X-Robots-Tag` contains `noindex`. |
| `fail-on-blocked` | `false` | Optionally fails when a named AI crawler token is blocked in robots.txt. |
| `check-edge-responses` | `false` | Optionally sends three separately labeled synthetic crawler requests. |
| `fail-on-restricted` | `false` | Optionally fails when an enabled synthetic response is restricted. |

## What the check reports

- homepage HTTP response
- robots meta and `X-Robots-Tag` directives
- canonical URL
- robots.txt policy for `OAI-SearchBot`, `GPTBot`, `OAI-AdsBot`, `Claude-SearchBot`, `ClaudeBot`, `Claude-User`, `PerplexityBot`, and `Google-Extended`
- optional synthetic homepage responses for three AI search-crawler user agents

Outputs include the compact policy result, indexability result, `noindex-found`, and the optional edge result.

## Evidence boundary

The check reads the public response, raw HTML, and headers without running browser JavaScript. Synthetic requests do not authenticate provider IP ranges. A clean run does not prove crawling, indexing, citation, ranking, or traffic.

GitHub schedules can be delayed during busy periods and may be disabled after prolonged repository inactivity. This Action checks one homepage. It is not Google Search Console, a dashboard, a real-crawler log, or a dedicated alerting service.

## Need a dedicated alert?

[Crawler Watch](https://actablesite.com/crawler-watch?utm_source=github&utm_medium=action&utm_campaign=crawler-watch) checks one public site every 15 minutes, confirms a changed state twice, and emails the evidence. It also watches `sitemap.xml` and `llms.txt`. The plan is $9 per month.

The free Action is complete without the managed service.

## Source and license

The wrapper uses the tested, MIT-licensed [`unitedideas/actablesite-check`](https://github.com/unitedideas/actablesite-check) stable `v1` release line. This repository is also MIT licensed.
