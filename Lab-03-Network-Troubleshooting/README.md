# Lab 3: Windows Network Troubleshooting

## Objective
Practice diagnosing network connectivity and DNS issues using built-in Windows command-line tools.

## Help Desk Scenario
An employee reports that their computer is connected to Wi-Fi but cannot access websites. The Help Desk technician must determine whether the issue involves the local network, internet connectivity, or DNS.

## Environment
- Operating System: Windows 11
- Connection: Wi-Fi
- Tools: Command Prompt, ipconfig, ping, nslookup, tracert
- Internet Service Provider: Sonic

## Steps Performed

### 1. Inspected Network Configuration
Command: `ipconfig`

Identified the following:
- IPv4 Address: 192.168.4.26
- Subnet Mask: 255.255.252.0
- Default Gateway: 192.168.4.1

**Finding:** The computer had a local IPv4 address and a configured default gateway.

### 2. Tested Local Network Connectivity
Command: `ping 192.168.4.1`

Results:
- Packets sent: 4
- Packets received: 4
- Packet loss: 0%
- Average latency: 5 ms

**Finding:** The computer successfully communicated with its default gateway.

### 3. Tested External IP Connectivity
Command: `ping 8.8.8.8`

Results:
- Packets sent: 4
- Packets received: 4
- Packet loss: 0%
- Average latency: 54 ms

**Finding:** The computer successfully reached an external IP address.

### 4. Tested DNS Resolution
Command: `nslookup google.com`

Results:
- DNS Server: ns1.sonic.net
- DNS Server Address: 50.0.1.1
- Resolved IPv4 Address: 142.251.219.14

**Finding:** DNS successfully resolved google.com into an IP address.

### 5. Traced the Network Route
Command: `tracert 8.8.8.8`

Results:
- First hop: 192.168.4.1
- Destination: 8.8.8.8
- Destination reached at hop 16
- Several intermediate hops did not respond

**Finding:** The trace successfully reached its destination. Intermediate timeouts did not prevent connectivity.

## Troubleshooting Conclusion
The computer successfully communicated with its local gateway, reached an external IP address, and resolved a domain name.

No basic network connectivity or DNS resolution failure was observed during testing.

If the employee continued experiencing website access issues, further troubleshooting would include checking the affected browser, website availability, and application-specific settings.

## Key Takeaways
- `ipconfig` displays local network configuration.
- `ping` tests whether a destination responds to network requests.
- `nslookup` helps diagnose DNS resolution.
- `tracert` displays network hops toward a destination.
- A traceroute timeout does not always indicate a network failure.
- Troubleshooting should proceed systematically from local connectivity to external services.

## Skills Demonstrated
- Windows 11 network troubleshooting
- TCP/IP configuration inspection
- Local and external connectivity testing
- DNS troubleshooting
- Network route analysis
- Technical documentation
