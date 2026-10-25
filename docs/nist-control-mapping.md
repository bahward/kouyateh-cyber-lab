# NIST control mapping

How the lab's controls map to NIST SP 800-53 and NIST CSF 2.0. Identifiers are my working mapping and should be checked against the current NIST publications. Rows marked *planned* have no evidence yet.

| Lab control | NIST SP 800-53 | NIST CSF 2.0 | Evidence / status |
|---|---|---|---|
| pfSense segmentation and deny-by-default rules | SC-7, AC-4 | PR.IR-01 | Rules on four segments; allow and deny tests passed 2026-10-25 |
| Scoped admin access from ADMIN_HOST only (GUI, SSH, Wazuh 443) | AC-3, AC-6 | PR.AA-05 | KE_CORP rules; GUI reachable only on 10.10.10.1:443 |
| Block-and-log default deny on every segment | AU-2, AU-12 | DE.CM-01 | Logging configured; log review not yet verified |
| Closed ports and least functionality | CM-6, CM-7 | PR.PS-01 | Firewall SSH closed; unused KE-APP01 ports dropped; GUI port 80 decision pending |
| Time-boxed exceptions (temporary apt rule) | CM-3, CM-7 | PR.PS-01 | Exceptions log: created and deleted 2026-10-25 |
| Rollback snapshots | CM-2, CP-10 | PR.PS-01 | Snapshots taken before VM changes |
| Active Directory groups and least privilege | AC-2, AC-6, IA-2 | PR.AA-01, PR.AA-03, PR.AA-05 | Planned |
| Group Policy and host hardening | CM-2, CM-6 | PR.PS-01 | Planned |
| Wazuh log collection and alerting | AU-6, SI-4 | DE.CM-01, DE.CM-09, DE.AE-02 | Planned |
| Sysmon endpoint telemetry | AU-2, AU-12, SI-4 | PR.PS-04, DE.CM-09 | Planned |
| Scoped Kali assessment with temporary rule | CA-8, RA-5 | ID.RA-01 | Planned |
| Restorable backups separate from snapshots | CP-9 | PR.DS-11 | Planned |
| Redacted configuration exports | SA-5, SC-28 | PR.DS-01 | Planned |
| Incident triage of a test alert | IR-4, IR-5 | RS.AN-03, RS.MA-02 | Planned |
