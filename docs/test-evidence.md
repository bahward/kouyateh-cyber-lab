# Test evidence

**Date:** 2026-10-25
**Source:** my host's admin adapter, 10.10.10.2 (Corporate segment)
**Method:** read-only network tests (connection attempts, ping, DNS lookups). No configuration was changed during testing.

## Firewall (KE-FW01)

| Test | Expected | Result |
|---|---|---|
| GUI on 10.10.10.1:443 | Reachable | Pass: HTTP 200 |
| GUI on 10.10.20.1, 10.10.30.1, 10.10.40.1 port 443 | Not reachable | Pass: timed out |
| Firewall SSH (22) | Closed | Pass: timed out |
| Ping to firewall | Replies | Pass after I added the ICMP rule (was 100% loss before) |
| DNS through the firewall | Resolves | Pass |

## KE-APP01 (10.10.20.10)

| Test | Expected | Result |
|---|---|---|
| SSH (22) from admin host | Connects | Pass: OpenSSH_9.6p1 Ubuntu banner returned |
| Ping | Blocked | Pass: 100% loss |
| 3389 and 8080 | Dropped | Pass: timed out (default deny) |
| 80 and 443 | Allowed by firewall | Pass: connection refused by the VM (no web server yet), so the firewall let it through |

## Not tested yet

- Firewall log review.
- Outbound traffic from KE-APP01. The only observation so far: after I deleted the temporary rule, apt could no longer reach the Ubuntu mirrors.
- Kali / KE_TEST isolation (Kali is still on NAT).
- Wazuh paths (Wazuh not built).
