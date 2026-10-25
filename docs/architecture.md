# Architecture

## Segments and addressing

| Segment | pfSense interface | VMware network | Subnet | Gateway |
|---|---|---|---|---|
| Internet side | WAN | VMnet8 (NAT) | DHCP from VMware NAT | n/a |
| Corporate | KE_CORP (LAN) | VMnet2 | 10.10.10.0/24 | 10.10.10.1 |
| Servers | KE_SERVERS | VMnet3 | 10.10.20.0/24 | 10.10.20.1 |
| Security | KE_SECURITY | VMnet4 | 10.10.30.0/24 | 10.10.30.1 |
| Testing | KE_TEST | VMnet5 | 10.10.40.0/24 | 10.10.40.1 |

The firewall is KE-FW01 (pfSense 2.9.0, hostname ke-fw01.kouyateh.test).

## VM roles

| VM | OS | Segment | Address | Status |
|---|---|---|---|---|
| KE-FW01 | pfSense 2.9.0 | All | .1 on each segment | Built |
| KE-APP01 | Ubuntu Desktop | Servers | 10.10.20.10/24 (static, gateway and DNS 10.10.20.1) | Built |
| KE-TEST01 | Kali Linux | Testing | 10.10.40.10 | VM exists, still on NAT; move planned |
| KE-DC01 | Windows Server (AD, DNS) | Corporate | 10.10.10.10 | Planned |
| KE-WS01 | Windows 11 | Corporate | 10.10.10.20 | Planned |
| KE-SIEM01 | Wazuh | Security | 10.10.30.10 | Planned |

A temporary host adapter on VMnet2 (my admin host, 10.10.10.2, reserved with a DHCP static mapping) is my management path into pfSense. I will remove it at the end of the build.

## Design decisions

- **Deny by default.** I deleted the default allow-all rules. Every interface ends with a block rule that logs, and I only add the traffic I need.
- **Least privilege.** Administration (the firewall GUI, SSH to servers, the Wazuh dashboard) is only allowed from one source, the `ADMIN_HOST` alias. The firewall GUI only answers on the Corporate address, not on the other gateways.
- **No internet for internal segments.** Internal segments only get DNS from the firewall. If a VM needs updates, I add a temporary, dated rule and remove it afterwards.
- **Kali isolation.** The Testing segment is blocked from Corporate and everything else by default. During an assessment, I will add one temporary, labelled rule that lets Kali reach only KE-APP01.
- **Only the firewall touches NAT.** No lab VM should use a bridged or NAT adapter (Kali currently breaks this; see risk R-01).
- **Snapshots before changes.** I take a VMware snapshot before every significant change.
