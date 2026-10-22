# Lab 07 — MSP Wi-Fi, DHCP, and VLAN Troubleshooting

**Lab Type:** Guided MSP Help Desk Simulation  
**Difficulty:** Medium  
**Ticket Status:** Resolved  
**Environment:** Simulated corporate Windows network  
**Role:** Tier 1 Help Desk Technician

---

## 1. Objective

Investigate a simulated MSP Help Desk ticket involving multiple employees who could connect to the company Wi-Fi network but could not access the internet or business applications.

The objective was to practice diagnosing network connectivity problems, identifying DHCP failures, comparing working and nonworking devices, escalating infrastructure issues, and verifying that the problem was resolved.

## 2. Help Desk Scenario

An employee named David reported that his Windows laptop was connected to office Wi-Fi but could not access the internet, Outlook, or the company CRM.

A second employee, Michael, experienced the same problem. Another employee, Sarah, had a working connection.

The issue affected employees who needed access to business applications to perform their jobs.

**Initial symptoms:**

- Wi-Fi showed as connected.
- Internet access was unavailable.
- Outlook could not connect.
- The company CRM was inaccessible.
- Multiple employees were affected.

**Goal:** Determine whether the problem originated from the affected computers, DHCP configuration, wireless infrastructure, or another network component.

## 3. Tools Used

| Tool / Command | Purpose |
|---|---|
| `ipconfig /all` | Examine IP addresses, DHCP settings, default gateway, and DNS configuration |
| `ipconfig /renew` | Request a new DHCP lease |
| `tracert 8.8.8.8` | Investigate connectivity and routing to an external destination |
| `netsh wlan show interfaces` | Identify wireless connection details and the connected access point |
| Network management dashboard | Simulated administrator inspection of access points and switch ports |
| Approved network documentation | Verify the intended VLAN configuration |

All command outputs and infrastructure details in this exercise were provided as part of the simulation, rather than collected from a production company network.

## 4. Troubleshooting Steps

### Step 1 — Gather information

I started by investigating the reported symptoms and determining whether the problem affected one employee or multiple employees.

David reported being connected to Wi-Fi but unable to use internet-dependent applications.

I learned that Michael was experiencing the same issue, while Sarah's computer was working normally.

**Assessment:** Because multiple employees were affected, I considered the possibility of a shared network infrastructure problem.

### Step 2 — Examine IP configuration

I requested:

```powershell
ipconfig /all
```

The simulated results showed that David's laptop had:

```text
IPv4 Address:     169.254.82.17
Subnet Mask:      255.255.0.0
Default Gateway:  Not assigned
DHCP Server:      Not listed
DNS Servers:      Not listed
```

**Finding:** The `169.254.x.x` address indicated that Windows had assigned an Automatic Private IP Addressing (APIPA) address.

This suggested that the laptop had not successfully obtained an IPv4 lease through DHCP.

The missing default gateway also explained why normal communication with external networks was unavailable.

### Step 3 — Investigate network routing

I requested:

```powershell
tracert 8.8.8.8
```

The simulated trace failed immediately with a transmit error.

**Finding:** The result was consistent with the computer lacking a usable route to the destination.

It supported the existing connectivity diagnosis but did not identify the underlying cause of the DHCP failure.

### Step 4 — Attempt DHCP renewal

After receiving guidance on the appropriate command, I requested:

```powershell
ipconfig /renew
```

The simulated response indicated that the DHCP server could not be contacted and the request timed out.

**Finding:** David's laptop was unable to obtain a DHCP lease.

### Step 5 — Compare working and nonworking computers

I requested DHCP renewal tests on Sarah's and Michael's laptops.

| Employee | DHCP Result | Network Status |
|---|---|---|
| David | Failed | No internet |
| Michael | Failed | No internet |
| Sarah | Successful | Internet working |

Sarah received a valid address on the `192.168.1.x` network.

David and Michael could not successfully renew their leases.

**Assessment:** The difference suggested that the DHCP service was reachable from part of the office network but not from the affected employees' connections.

### Step 6 — Identify the connected wireless access points

I investigated the wireless connections using:

```powershell
netsh wlan show interfaces
```

The simulated results identified the access points used by each employee.

| Employee | Access Point | Connection |
|---|---|---|
| Sarah | AP-01 | Working |
| David | AP-02 | Not working |
| Michael | AP-02 | Not working |

David's laptop showed a strong wireless signal despite having no usable internet connection.

**Finding:** Both affected employees were connected through AP-02, while the working employee was connected through AP-01.

This narrowed the investigation toward AP-02 or its upstream network connection.

### Step 7 — Escalate the infrastructure investigation

Because the problem appeared to involve managed network equipment, I coordinated with the simulated network administrator.

The administrator inspected AP-02 and confirmed:

- The access point was online.
- Wi-Fi broadcasting was active.
- The Ethernet uplink was connected.
- DHCP responses were not reaching affected clients.
- AP-02 was connected through switch port 12.

**Assessment:** The access point had an active physical connection, but clients were still unable to obtain network configuration.

### Step 8 — Compare switch configurations

The administrator examined the switch port configuration.

| Configuration | AP-01 (Working) | AP-02 (Affected) |
|---|---|---|
| Switch port | Port 8 | Port 12 |
| Port status | Up | Up |
| VLAN assignment | VLAN 10 | VLAN 20 |
| DHCP access | Working | Failing |

**Finding:** AP-02 was connected through a different VLAN than the working access point.

This suggested a possible VLAN configuration mismatch.

I did not immediately recommend changing the port because the correct configuration needed to be confirmed.

### Step 9 — Verify the approved configuration

I requested verification against the approved network documentation.

The administrator confirmed that AP-02's switch port should be assigned to VLAN 10 rather than VLAN 20.

Additional checks confirmed that:

- DHCP was operational on VLAN 10.
- VLAN 20 did not provide the expected DHCP service.
- Port 12 connected to AP-02.
- The approved configuration required VLAN 10.
- A rollback option was available.

**Finding:** The VLAN mismatch was confirmed as the cause of the connectivity problem in the simulated environment.

### Step 10 — Coordinate the authorized correction

After the configuration was verified and approval was obtained, the simulated network administrator changed switch port 12 from VLAN 20 to VLAN 10.

I did not personally perform the network switch configuration.

My role was to investigate the issue, identify the likely cause, request verification, and coordinate the resolution.

### Step 11 — Renew DHCP leases

Following the correction, David and Michael reconnected and renewed their network configurations.

```powershell
ipconfig /renew
ipconfig /all
```

The simulated results showed:

| Setting | David | Michael |
|---|---|---|
| IPv4 address | 192.168.1.110 | 192.168.1.111 |
| Subnet mask | 255.255.255.0 | 255.255.255.0 |
| Default gateway | 192.168.1.1 | 192.168.1.1 |
| DHCP lease | Successful | Successful |

Both computers now had usable network configurations.

### Step 12 — Verify business applications

I confirmed that both employees could access the services needed for their work.

**Verification checklist:**

- [x] DHCP lease successfully obtained
- [x] Valid IPv4 address assigned
- [x] Default gateway assigned
- [x] DNS configuration received
- [x] Internet browsing restored
- [x] Outlook connected successfully
- [x] Email sending and receiving worked
- [x] Company CRM opened successfully
- [x] Both affected employees confirmed resolution

The simulated ticket was marked resolved after user verification.

## 5. Findings and Assessment

The investigation established that the issue was not simply weak Wi-Fi signal strength or an isolated computer problem.

The key findings were:

1. David's computer received an APIPA address, indicating an unsuccessful DHCP configuration process.
2. DHCP renewal failed for both affected employees.
3. Sarah's working computer successfully received network configuration.
4. Both affected employees were connected through AP-02.
5. AP-02 was online and physically connected, but its clients could not reach the expected DHCP service.
6. The connected switch port was assigned to VLAN 20 instead of the approved VLAN 10.

**Root Cause:** Incorrect VLAN assignment on switch port 12, which served AP-02.

This prevented affected clients from reaching the expected DHCP service and obtaining usable network configurations.

## 6. Conclusion / Resolution

The simulated network administrator corrected switch port 12's VLAN assignment from VLAN 20 to VLAN 10 after confirming the approved configuration and obtaining authorization.

David and Michael then successfully renewed their DHCP leases.

Both employees regained access to the internet, Outlook, and the company CRM.

**Final Ticket Status:** Resolved

This exercise demonstrated the importance of narrowing an issue through evidence rather than making configuration changes based on an early assumption.

## 7. What I Learned

### DHCP troubleshooting

I learned that an address beginning with `169.254` can indicate that a Windows computer could not obtain an IPv4 lease from DHCP.

I also practiced using `ipconfig /all` to inspect network settings and `ipconfig /renew` to request a lease.

### Comparing affected users

I learned how comparing a working employee with affected employees can help identify whether a problem is isolated or shared.

### Wireless troubleshooting

I learned that a computer can have a strong Wi-Fi signal and still lack network connectivity.

The `netsh wlan show interfaces` command helped identify which access point each employee was using.

### VLAN concepts

I learned that an incorrect VLAN assignment can prevent devices from reaching the network services they need, even when the switch port and access point appear operational.

### Escalation and change management

I practiced identifying when an infrastructure issue should be escalated to a network administrator.

I also reinforced the importance of verifying approved configurations, obtaining authorization, and considering rollback options before making changes.

### Areas for improvement

I needed guidance recalling some diagnostic commands, particularly `ipconfig /renew`.

My next goal is to improve command recall and independently select appropriate troubleshooting steps.

## 8. Screenshots / Evidence

**Evidence Type:** Guided text-based simulation

This lab did not involve configuring an actual enterprise switch or troubleshooting real employees' computers.

The diagnostic outputs, access point information, VLAN assignments, and resolution results were supplied during the simulation.

The following evidence was reviewed during the exercise:

| Evidence | Observation |
|---|---|
| Initial `ipconfig /all` | APIPA address and missing network configuration |
| `tracert` result | External route test failed |
| `ipconfig /renew` | DHCP request timed out |
| Working computer comparison | Sarah successfully obtained a DHCP lease |
| Wireless interface comparison | Both affected employees used AP-02 |
| Switch configuration comparison | VLAN 20 differed from approved VLAN 10 |
| Post-resolution configuration | Valid DHCP leases obtained |
| Application verification | Internet, Outlook, and CRM restored |

**Evidence note:** These are simulated troubleshooting records, not screenshots captured from a production network. No screenshots are claimed where none were collected.

## 9. Next Steps

To build on this exercise, I plan to:

- Practice Windows networking commands without relying on hints.
- Strengthen my understanding of DHCP, DNS, VLANs, and default gateways.
- Continue investigating realistic multi-user MSP incidents.
- Practice identifying when to escalate issues to specialized teams.
- Improve the clarity and consistency of my ticket documentation.
- Progress to a simulated Microsoft 365 sign-in troubleshooting ticket.

---

**Portfolio Disclaimer:** This was a guided, simulated MSP Help Desk exercise. Employee identities, network equipment, diagnostic results, and configuration changes were fictional. No production company systems were accessed or modified.
