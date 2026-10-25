# Risk register

Last updated 2026-10-25. Only verified facts are recorded; anything not yet done is marked as planned.

| ID | Risk | Likelihood | Impact | Treatment | Status |
|---|---|---|---|---|---|
| R-01 | Kali is on VMware NAT, not the isolated Testing network, so assessment traffic could reach the host network or internet | High | High | Snapshot, then move Kali to VMnet5 (10.10.40.10/24) with a temporary, labelled, dated rule to KE-APP01 only | Open (next step) |
| R-02 | Ubuntu sat on VMnet2 (Corporate) as a temporary management exception | Medium | Medium | Moved to VMnet3 (KE_SERVERS) as KE-APP01, static 10.10.20.10/24, gateway and DNS 10.10.20.1 on 2026-10-25 | Closed |
| R-03 | pfSense configuration beyond the setup wizard was untested | Medium | High | Rules applied on all four segments and read-only tests passed 2026-10-25. Remaining: detach the installer ISO, set up log review, decide on port 80, test KE_TEST and Wazuh paths once built | In progress |
| R-04 | Default pfSense LAN allow-all rule weakened segmentation | Medium | High | Deleted the allow-all rules and replaced them with scoped rules ending in default deny and log (2026-10-25) | Closed |
| R-05 | Snapshots alone are not a backup | Medium | Medium | KE-APP01 snapshots taken before changes. Planned: restorable backups and redacted config exports | Partly done |
| R-06 | Old Debian, Kali and Ubuntu VMs from 2025 share the host and could be mistaken for lab assets | Medium | Low | Inventory them; archive or delete deliberately | Planned |
| R-07 | Credentials or personal data could end up in VM notes, chats or evidence | Medium | High | Policy: redact before saving anything. Implementation not yet verified | Policy only |
| R-08 | Kali and an older Kali VM share one disk file, so running both risks corruption | Low | Medium | Never power both on together; give the lab Kali its own disk | Open |
| R-09 | Temporary VMnet2 host adapter (admin host 10.10.10.2) connects my host to the Corporate segment | Medium | Medium | Remove when the rules and VM moves are done | Open, temporary |
