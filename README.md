# Aidelly status

Uptime monitoring and the public status page for [Aidelly](https://www.aidelly.ai), published at **[status.aidelly.ai](https://status.aidelly.ai)**.

Built on [Upptime](https://upptime.js.org): GitHub Actions runs the checks, GitHub Issues records incidents, GitHub Pages serves the page. **Nothing here runs on Aidelly's own infrastructure**, which is the point. A status page hosted alongside the product goes dark in exactly the incident it exists for.

<!--start: status pages-->
<!--end: status pages-->

## What is monitored, and what each check proves

**Aidelly services.** These carry the Agency SLA.

| Component | Probe | What a failure means |
|---|---|---|
| App and dashboard | `/api/health` | The app is unreachable, or the database is not answering |
| Publishing | `/api/health/publishing` | Scheduled posts that were due have not gone out |
| Public API | `/api/public/v1/posts`, expects 401 | The API is down. 401 is the healthy answer: route up, auth enforced |
| MCP server | `/api/mcp`, expects 401 | Agent integrations cannot reach us |

The health endpoints return a single status word and nothing else. No versions, no configuration, no counts. They are public by design so the monitor can reach them from every region, and they are deliberately uninteresting to anyone else.

**Platform APIs.** Displayed so customers can tell whether a publishing problem is ours or the network's, since "why didn't my post go out" is usually answered by a degraded platform rather than by us.

Instagram and Facebook, Threads, LinkedIn, X, TikTok, YouTube, Pinterest, Google Business Profile, Bluesky, Mastodon.

Each is probed at its real API host. An unauthenticated request to a healthy API returns a deterministic 400, 401 or 403, so a 5xx or a timeout is a genuine signal.

> **These checks detect outright outages and cannot see partial degradation.** A green platform is not a guarantee that every endpoint behind it is healthy. Rate limiting, slow media processing, and per-endpoint faults will not show here.

## What is deliberately not monitored

- **The marketing site.** Nobody opens a status page to ask whether the pricing page loads, and including it inflates the incident surface for no one's benefit.
- **Inbox sync.** There is no honest signal for it yet: the sync table records timestamps but has no success or failure discriminator, so any indicator would be guessing.
- **Sustained publish failure.** Posts that exhaust their retries move to a terminal failed state that no probe watches, so a long outage shows red while the backlog churns and then goes green as it drains. Tracked in [Aidelly/aidelly#2686](https://github.com/Aidelly/aidelly/issues/2686).

## Measurement

Checks run every **five minutes**. That interval is part of the SLA definition, not an implementation detail: at 99.9% the monthly allowance is roughly 43 minutes, and five-minute sampling cannot observe a three-minute outage. That is generous to us, which is why it is published rather than left to be discovered during a credit dispute.

Service credits apply to **App and dashboard** and **Publishing** only. Platform APIs are shown for transparency and are explicitly out of scope, since we cannot be liable for a social network's downtime.

No history is backfilled. This page starts empty and fills with real data.

## Reporting a problem

Security issues: **security@aidelly.com**, or see [security.txt](https://www.aidelly.ai/.well-known/security.txt).
Everything else: [aidelly.ai/contact](https://www.aidelly.ai/contact).

## Licence

Upptime is MIT licensed. See [LICENSE](./LICENSE).
