# Lab 06 — Simulated MSP DNS Troubleshooting Ticket

## Overview
Simulated a Tier 1 MSP Help Desk ticket involving an employee unable to access a company reporting website. Investigated the issue by comparing network configurations between working and nonworking computers.

## Reported Issue
An employee named Sarah could not access a company reporting website while another employee could.

## Troubleshooting Steps
1. Gathered information about the reported connectivity issue.
2. Compared the working and affected computers.
3. Used `ipconfig /all` to inspect IP, gateway, DHCP, and DNS configuration.
4. Used `nslookup` and `ping` to investigate connectivity and name resolution.
5. Identified a manually configured DNS server (`192.168.1.10`) on the affected computer.
6. Obtained authorization before changing the DNS configuration.
7. Changed DNS assignment to automatic (DHCP).
8. Verified that the employee could access the reporting website.

## Root Cause
The affected computer was configured to use a manually specified DNS server instead of receiving the appropriate DNS settings automatically through DHCP.

## Resolution
With authorization, changed the computer's DNS configuration to automatic assignment through DHCP.

## Verification
Confirmed that the company reporting website was accessible after the change.

## Tools and Commands
- `ipconfig /all` — Inspected IP, DHCP, gateway, and DNS configuration.
- `nslookup` — Investigated DNS name resolution.
- `ping` — Tested network connectivity.
- Windows network settings — Updated DNS assignment.

## Skills Demonstrated
- Tier 1 ticket investigation
- Working versus nonworking device comparison
- DNS and DHCP troubleshooting
- Windows network configuration
- Change authorization
- Resolution verification and ticket documentation

**Lab type:** Guided, simulated MSP Help Desk ticket. No production company systems were accessed.
