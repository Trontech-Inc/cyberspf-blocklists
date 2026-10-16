# CyberSPF Public Blocklists

Free, open threat blocklists published by **[CyberSPF](https://cyberspf.com)**.
Updated hourly. Licensed [CC BY 4.0](#license).

## Lists

### `decoy-ips.txt` — Decoy sensor feed

Source IPs caught probing CyberSPF **decoy (honeypot) infrastructure** — systems
that exist only to attract scanners and attackers. Anything that reaches them is
almost certainly hostile, so this is a small, high-confidence, low-false-positive
feed. It's a rolling window: stale entries age out automatically.

Plain one-IP-per-line, IPv4 and IPv6, with a `#` comment header.

**Subscribe to this URL:**

```
https://raw.githubusercontent.com/Trontech-Inc/cyberspf-blocklists/main/decoy-ips.txt
```

> These are **IP addresses**, for firewall tools — not a domain blocker.

## Using it

**pfSense / OPNsense (pfBlockerNG):** add an IPv4 (and a separate IPv6) alias
with the URL above as the source and the action set to Deny.

**ipset + iptables (Linux):**

```sh
ipset create cyberspf-decoy hash:ip family inet -exist
curl -fsS https://raw.githubusercontent.com/Trontech-Inc/cyberspf-blocklists/main/decoy-ips.txt \
  | grep -vE '^#|^$' | grep -F '.' \
  | while read -r ip; do ipset add cyberspf-decoy "$ip" -exist; done
iptables -I INPUT -m set --match-set cyberspf-decoy src -j DROP
# IPv6: repeat with `family inet6`, the lines containing ':', and ip6tables.
```

**CrowdSec:** add it as a URL-based blocklist source.

Re-fetch on a schedule (the list changes over time); a daily or hourly cron is fine.

## What is and isn't here

Only CyberSPF's **own observations** are published — the decoy feed above. We do
**not** redistribute third-party feeds (Spamhaus, abuse.ch, Blocklist.de, Tor
lists, etc.); their licenses don't permit it, and you should get those from their
sources.

## License

The blocklist data in this repository is licensed under
**[Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/)**.
Free for any use, including commercial, with attribution to
**CyberSPF (https://cyberspf.com)**. See [`LICENSE`](LICENSE).
