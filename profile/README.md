# Hyrule Networks (AS215932)

Welcome to the public operations repository of **Hyrule Networks (AS215932)** —
a solo-built, multi-homed, IPv6-first Internet Service Provider developed live on
GitHub.

We believe ISP infrastructure should be documented, reproducible, and open to
contribution. Everything here runs in production, is version-controlled, and is
shared under permissive terms.

## Quick facts

- **ASN:** [AS215932](https://www.peeringdb.com/asn/215932)
- **NOC:** noc@as215932.net
- **Website:** https://as215932.net
- **Peering policy:** Open — see [PeeringDB](https://www.peeringdb.com/asn/215932)
- **License:** CC0 / public domain unless otherwise noted

## Hyrule Cloud — agentic infrastructure, paid per request via x402

**[Hyrule Cloud](https://hyrule.host)** is full-stack network infrastructure for
AI agents, running on AS215932 and billed per request in USDC on Base, Polygon,
or Arbitrum via [x402](https://x402.org). Agents discover it via the x402 Bazaar or the manifest
below, pay per call, and get bare VMs, DNS, network intelligence, and anonymous
egress — no account, no KYC, no dashboard required.

- **Compute** — bare IPv6-native VMs with SSH, automatic HTTPS subdomains, and custom domains (coming soon).
- **Domains & DNS** — *coming soon.* Domain registration and managed DNS are not yet launched; every VM already gets a free `*.deploy.hyrule.host` subdomain.
- **Network intelligence** — BGP/routing over AS215932's own tables plus RouteViews/RIPE RIS, IP geolocation/ASN/reputation, DNS (lookup, propagation, DNSSEC, record recommendations), RDAP/WHOIS, web reachability and deep TLS grading, MXToolbox-compatible mail deliverability, port and NAT/CGNAT reachability, and VoIP/SIP diagnostics.
- **Network proxy** — outbound requests over Direct, Tor, I2P, or Yggdrasil.

| Surface | URL |
|---------|-----|
| Website | https://hyrule.host |
| x402 manifest | https://cloud.hyrule.host/.well-known/x402.json |
| OpenAPI | https://cloud.hyrule.host/openapi.json |
| Agent guide (`llms.txt`) | https://hyrule.host/llms.txt |

### Golden path (curl)

Every paid endpoint answers an unpaid request with a `402` carrying the price and
payment requirements (`PAYMENT-REQUIRED` header); sign it with any x402-aware
client and replay with the `Payment-Signature` header:

```bash
# 1. Ask — get a 402 with the price + payment requirements
curl -si https://cloud.hyrule.host/v1/dns/lookup \
  -H 'content-type: application/json' \
  -d '{"name":"example.com","type":"AAAA"}'
# HTTP/1.1 402 Payment Required   (USDC on Base, Polygon, or Arbitrum)

# 2. Pay — replay the same request with the signed payment payload
curl -si https://cloud.hyrule.host/v1/dns/lookup \
  -H 'content-type: application/json' \
  -H 'Payment-Signature: <signed-payment-payload>' \
  -d '{"name":"example.com","type":"AAAA"}'
# HTTP/1.1 200 OK   -> diagnostic evidence
```

VM provisioning is asynchronous: pay `POST /v1/vm/create` → `202` with a public
`status_url` (`GET /v1/vm/{id}/status` — no token) plus a save-once
`management_url`. Poll the `status_url` until `status` is `ready` and it reports
`{ ipv6, hostname }`, then `ssh root@<hostname>`. Full VM details and lifecycle
actions (extend, destroy) go through the token-bearing `management_url`.

Typical builds complete in about 60 seconds (the live average is shown on
[hyrule.host](https://hyrule.host)). Runtime is prepaid for 1–365 days and can be
extended; expired VMs are suspended and destroyed after a 24-hour grace period.

### Pricing

Payment settles in **USDC on Base (`eip155:8453`), Polygon (`eip155:137`), or
Arbitrum (`eip155:42161`)** via the x402 *exact* scheme. VM checkout also accepts
native **XMR**. `GET https://cloud.hyrule.host/v1/payments/networks` lists the
currently enabled rails.

**Compute** (per day; free auto-subdomain under `deploy.hyrule.host`)

| Size | vCPU / RAM / Disk | Price |
|------|-------------------|-------|
| xs (1C-1G-10G) | 1 / 1 GB / 10 GB | $0.20/day |
| sm (1C-2G-20G) | 1 / 2 GB / 20 GB | $0.40/day |
| md (2C-4G-20G) | 2 / 4 GB / 20 GB | $0.60/day |
| lg (4C-4G-40G) | 4 / 4 GB / 40 GB | $0.80/day |

Custom sizes up to 4 vCPU / 8 GB RAM / 40 GB disk, with add-ons at $0.10 per
vCPU/day, $0.15 per GB RAM/day, and $0.05 per 10 GB disk/day.

**Network intelligence** (per request) — from **$0.001**; a representative sample:

| Endpoint | Price |
|----------|-------|
| `POST /v1/dns/lookup` | $0.001 |
| `POST /v1/ip/lookup` | $0.003 |
| `POST /v1/bgp/lookup` | $0.005–$0.01 |
| `POST /v1/web/check` | $0.005 |
| `POST /v1/mx/check` | $0.005 |
| `POST /v1/web/tls/deep` | $0.10 |

**Network proxy** (per request)

| Route | Price |
|-------|-------|
| direct | $0.01 |
| yggdrasil | $0.03 |
| tor | $0.05 |
| i2p | $0.05 |

**Domains & DNS**

| Item | Price |
|------|-------|
| Auto subdomain (`*.deploy.hyrule.host`) | free |
| Domain registration & managed DNS | coming soon |

`GET https://cloud.hyrule.host/v1/pricing` returns the live VM, auto-subdomain,
and proxy prices. The complete per-endpoint catalog — including every network-intel
lookup — lives in the
[x402 manifest](https://cloud.hyrule.host/.well-known/x402.json) (`minPrice` per
resource), which is also what the Bazaar and x402 clients read.

## Repository map

| Repository | Purpose |
|------------|---------|
| [`network-operations`](https://github.com/AS215932/network-operations) | Core infrastructure record: configs, docs, deployment runbook, and peering requests. |
| [`hyrule-mcp`](https://github.com/AS215932/hyrule-mcp) | Live MCP diagnostic tools for routing, firewall, host, and packet-path inspection. |
| [`engineering-loop`](https://github.com/AS215932/engineering-loop) | Autonomous development loop for infrastructure changes — plans, implements, and gates draft PRs. |
| [`as215932.net`](https://github.com/AS215932/as215932.net) | Static website with ASN facts, topology, peering policy, and weathermap. |
| [`noc-agent`](https://github.com/AS215932/noc-agent) | Autonomous NOC agent: alert intake, incident analysis, and operator review. |
| [`hyrule-cloud`](https://github.com/AS215932/hyrule-cloud) | Agentic VPS hosting API with x402 payments on AS215932. |
| [`hyrule-web`](https://github.com/AS215932/hyrule-web) | Main branded website for Hyrule Networks. |

## Peering with us

We maintain an open peering policy at every IXP where we have a presence.

**Requirements:**

- Valid PeeringDB entry
- IRR objects registered
- 24/7 NOC contact
- RPKI ROA configured

Open a peering request in [`network-operations`](https://github.com/AS215932/network-operations/issues/new?labels=peering) or contact us via PeeringDB.

## How to contribute

- Browse open issues in `network-operations` to see what we're working on.
- Report bugs or suggest improvements via GitHub Issues.
- Submit peering requests using the `peering` label.
- Read our [Contributing Guide](https://github.com/AS215932/network-operations/blob/main/CONTRIBUTING.md).

## Stay updated

Watch the `network-operations` repository for the most frequent infrastructure
updates, or follow the commit history to see the network evolve in real time.

> *Building the Internet, one BGP session at a time.*
