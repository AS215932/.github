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
AI agents, running on AS215932 and billed per request in USDC on Base via
[x402](https://x402.org). Agents discover it via the x402 Bazaar or the manifest
below, pay per call, and get bare VMs, DNS, network intelligence, and anonymous
egress — no account, no KYC, no dashboard required.

- **Compute** — bare IPv6-native VMs with SSH, automatic HTTPS subdomains, and optional custom domains.
- **Domains & DNS** — domain registration (Openprovider) and DNS management.
- **Network intelligence** — BGP/routing over AS215932's own tables plus RouteViews/RIPE RIS, IP geolocation/ASN/reputation, DNS (lookup, propagation, DNSSEC, record recommendations), RDAP/WHOIS, web reachability and deep TLS grading, MXToolbox-compatible mail deliverability, port and NAT/CGNAT reachability, and VoIP/SIP diagnostics.
- **Network proxy** — outbound requests over Direct, Tor, I2P, or Yggdrasil.

| Surface | URL |
|---------|-----|
| Website | https://hyrule.host |
| x402 manifest | https://cloud.hyrule.host/.well-known/x402.json |
| OpenAPI | https://cloud.hyrule.host/openapi.json |
| Agent guide (`llms.txt`) | https://hyrule.host/llms.txt |
| Skills | [ClawHub](https://clawhub.ai) under the `hyrule-` prefix — `hyrule-cloud` (umbrella) and `hyrule-network-intel` |

### Golden path (curl)

Every paid endpoint answers an unpaid request with a `402` carrying the price and
payment requirements; sign it with any x402-aware client and replay with the
`X-PAYMENT` header:

```bash
# 1. Ask — get a 402 with the price + payment requirements
curl -si https://cloud.hyrule.host/v1/dns/lookup \
  -H 'content-type: application/json' \
  -d '{"name":"example.com","type":"AAAA"}'
# HTTP/1.1 402 Payment Required   (USDC on Base, eip155:8453)

# 2. Pay — replay the same request with the signed payment payload
curl -s https://cloud.hyrule.host/v1/dns/lookup \
  -H 'content-type: application/json' \
  -H 'X-PAYMENT: <signed-payment-payload>' \
  -d '{"name":"example.com","type":"AAAA"}'
# HTTP/1.1 200 OK   -> diagnostic evidence
```

VM provisioning is asynchronous: pay `POST /v1/vm/create` → `202` + `status_url` →
poll `GET /v1/vm/{id}` until it returns `{ ipv6, hostname, ssh }` → `ssh root@…`.

### Pricing

Payment settles in **USDC on Base (`eip155:8453`)** via the x402 *exact* scheme.
VM checkout may also accept BTC/XMR when the live payment catalog advertises native
rails.

**Compute** (per day; free auto-subdomain under `deploy.hyrule.host`)

| Size | vCPU / RAM / Disk | Price |
|------|-------------------|-------|
| xs | 1 / 1 GB / 10 GB | $0.05/day |
| sm | 1 / 1 GB / 20 GB | $0.10/day |
| md | 2 / 2 GB / 40 GB | $0.20/day |
| lg | 4 / 4 GB / 80 GB | $0.40/day |

**Network intelligence** (per request) — from **$0.001**; a representative sample:

| Endpoint | Price |
|----------|-------|
| `POST /v1/dns/lookup` | $0.001 |
| `POST /v1/ip/lookup` | $0.003 |
| `POST /v1/bgp/lookup` | $0.005 |
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
| Domain registration | from $6.00 |

Full, always-current price list: `GET https://cloud.hyrule.host/v1/pricing`.

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
