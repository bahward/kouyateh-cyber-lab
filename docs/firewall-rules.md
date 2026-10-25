# Firewall rules (KE-FW01, pfSense 2.9.0)

I entered and applied these rules by hand on 2026-10-25. The old default allow-all rules were deleted. Rules are evaluated top to bottom; the first match wins.

## Aliases

| Alias | Value | Use |
|---|---|---|
| ADMIN_HOST | 10.10.10.2 | My one approved admin source |
| KE_APP01 | 10.10.20.10 | Application server |
| KE_WAZUH | 10.10.30.10 | Wazuh server (planned) |
| WAZUH_PORTS | 1514, 1515 | Wazuh agent traffic |
| WEB_PORTS | 80, 443 | Web traffic |

## KE_CORP (Corporate, 10.10.10.0/24)

| # | Action | Source | Destination | Why |
|---|---|---|---|---|
| 0 | Anti-lockout (built in) | Any | Firewall GUI | pfSense safety rule so I cannot lock myself out |
| 1 | Pass TCP | ADMIN_HOST | KE_CORP address, 443 | GUI management only from my admin host, only on 10.10.10.1 |
| 2 | Pass ICMP echo | ADMIN_HOST | KE_CORP address | Ping for troubleshooting |
| 3 | Pass TCP | ADMIN_HOST | KE_APP01, 22 | SSH to the server only from the admin source |
| 4 | Pass TCP | KE_CORP net | KE_APP01, WEB_PORTS | Corporate users reach the web app |
| 5 | Pass TCP | KE_CORP net | KE_WAZUH, WAZUH_PORTS | Wazuh agent enrollment and logs (Wazuh not built yet) |
| 6 | Pass TCP | ADMIN_HOST | KE_WAZUH, 443 | Wazuh dashboard (not built yet) |
| 7 | Pass TCP/UDP | KE_CORP net | This firewall, 53 | DNS until the domain controller exists |
| 8 | Block, log | Any | Any | Default deny |

## KE_SERVERS (Servers, 10.10.20.0/24)

| # | Action | Source | Destination | Why |
|---|---|---|---|---|
| 1 | Pass TCP | KE_SERVERS net | KE_WAZUH, WAZUH_PORTS | Wazuh agents send logs to Wazuh |
| 2 | Pass TCP/UDP | KE_SERVERS net | This firewall, 53 | DNS |
| 3 | Block, log | Any | Any | No path back to Corporate or Testing, no internet |

## KE_SECURITY (Security, 10.10.30.0/24)

| # | Action | Source | Destination | Why |
|---|---|---|---|---|
| 1 | Pass TCP/UDP | KE_SECURITY net | This firewall, 53 | DNS |
| 2 | Block, log | Any | Any | Default deny; replies to Wazuh agents are handled by connection state |

## KE_TEST (Testing, 10.10.40.0/24)

| # | Action | Source | Destination | Why |
|---|---|---|---|---|
| 1 | Block, log | KE_TEST net | 10.10.10.0/24 | Explicitly protect Corporate (identities) |
| 2 | Block, log | Any | Any | Default deny, which also covers the Security segment |

During an assessment (planned), a temporary rule above these will let Kali reach only KE_APP01. It will be labelled with its purpose and a removal date, and deleted afterwards.

## Exceptions log

| Date | Rule | Scope | Status |
|---|---|---|---|
| 2026-10-25 | `TEMP 2026-10-25 APP01 apt install, remove after` (KE_SERVERS) | KE_APP01 outbound TCP on WEB_PORTS only | Created to install OpenSSH; deleted the same day |

## Open items

- Port 80 on 10.10.10.1 is still open (anti-lockout / redirect). Decision pending.
- Firewall log review is not set up yet.
