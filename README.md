# ZAPI — Cheap OpenAI-Compatible API Gateway

[Live](https://zapi.xoui.dpdns.org) · [Docs](https://zapi.xoui.dpdns.org/docs)

OpenAI-compatible API at a fraction of the cost. One endpoint, multi-pool routing,
automatic failover — change one line in your code, start saving today.

## Why

- **Drop-in OpenAI compat** — standard `/v1/chat/completions` + `/v1/models`. Works with OpenAI SDK, Cherry Studio, LangChain, any tool that speaks OpenAI.
- **Multi-pool failover** — requests route through a free-quota pool first, paid pool as fallback. One request never hangs because one upstream is down.
- **Dirt cheap** — free tier for casual use, $6/mo for serious projects. No surprise bills, no quota drama.

```python
# before
client = OpenAI(base_url="https://api.openai.com/v1", api_key="...")
# after
client = OpenAI(base_url="https://zapi.xoui.dpdns.org/v1", api_key="<your-zapi-key>")
```

## Pricing

| Plan | Price | Requests/day | Notes |
|------|-------|--------------|-------|
| Developer | Free | 50 | No signup, no credit card |
| Pro | $6/mo | 5,000 | Priority routing, usage dashboard |
| Scale | $25/mo | 25,000 | Dedicated upstream pool |

## Models

GLM · Kimi · DeepSeek family — flagship models on the free tier, more landing as upstreams open up.

## Quick start

1. GET a key: `https://zapi.xoui.dpdns.org` → "Start free" (auto-issued, free tier works immediately)
2. Point your OpenAI-compatible client at `https://zapi.xoui.dpdns.org/v1`
3. Done. If you want Pro/Scale, [contact us](https://zapi.xoui.dpdns.org) — payment onboarding in progress.

## Architecture

```
client ──> cloudflared tunnel ──> zapi gateway (this repo's sibling service)
                                      ├── free-quota upstream pool (auto-maintained accounts)
                                      └── paid fallback upstream
```

The gateway is a single Node.js service (`server.js`) with zero dependencies beyond Node
built-ins: key issuing, per-token daily rate limits, usage logging, multi-pool routing.
The live server intentionally is **not** published here — it holds upstream credentials
and admin keys. This repo is the public face: docs, pricing, examples.

## Status

- [x] OpenAI-compatible endpoint, key issuing, rate limiting, usage tracking
- [x] Multi-pool routing with failover
- [x] Landing + pricing page
- [ ] Payment webhook (Lemon Squeezy) — onboarding
- [ ] Public dashboard

## License

MIT
