# Lessons learned

Short notes on what went wrong and how I fixed it.

## DHCP static mapping inside the pool
I tried to reserve 10.10.10.101 for my admin host, but pfSense would not accept it because that address was inside the DHCP pool (10.10.10.100 to .199). I reserved 10.10.10.2 instead, outside the pool. Lesson: static mappings must sit outside the dynamic range.

## pfSense IPv6 errors
pfSense showed an IPv6 error related to DHCPv6 and router advertisements. My lab is IPv4 only, so I disabled DHCPv6 and RA and set IPv6 to None on the interface. Lesson: turn off what you are not using rather than leaving it half-configured.

## Kali on NAT broke my isolation plan
I found that my Kali VM was still on VMware NAT, not the isolated Testing segment. That means my KE_TEST isolation rules do not apply to it at all. I logged it as my top risk (R-01) instead of hiding it, and moving it is my next step.

## A temporary exception for Ubuntu
To get started, my Ubuntu VM sat on the Corporate network (VMnet2). I treated that as a documented temporary exception, then snapshotted it, moved it to the Servers network (VMnet3) as KE-APP01, and gave it a static address.

## Deny by default blocked my own install
The Servers segment has no internet by design, so `apt install openssh-server` failed. DNS worked, but web traffic was blocked. I added a temporary rule, named with the date and "remove after", allowing only KE_APP01 out on ports 80/443. I installed OpenSSH and deleted the rule the same day, then recorded it in the exceptions log. Lesson: a secure default will get in your way, and the answer is a narrow, time-boxed, documented exception, not opening everything.

## Snapshots before every change
Before each change to a VM I took a VMware snapshot, so I could always roll back.

## Review before applying, verify after
I review each change plan before I apply it, and I verify the result with a test instead of assuming it worked. Every firewall change in this lab was made by hand.
