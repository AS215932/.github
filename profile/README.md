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
