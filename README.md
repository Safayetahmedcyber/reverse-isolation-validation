# Reverse Isolation Validation

## Objective
Confirm bidirectional network isolation between a laptop and a mobile device connected to separate Wi-Fi networks (main and guest) by analyzing ICMP traffic and ARP behavior in Wireshark.

## About
This project validates network isolation between devices on separate Wi-Fi networks (main and guest) by analyzing ICMP traffic with Wireshark and ping. It captures and interprets packets to determine if devices can communicate across isolated networks. Tests include VPN tunneling to evaluate its effect on traffic visibility and isolation enforcement.

## Environment Setup
- Laptop: Windows 11, Wireshark v4.0
- Mobile: Android 13
- Router: TP-Link Archer AX55
- VPN: ProtonVPN (WireGuard protocol)

## Test Procedure
1. Connect laptop to main Wi-Fi, mobile to guest Wi-Fi.
2. Run ICMP ping from mobile to laptop.
3. Capture traffic on laptop using Wireshark.
4. Repeat in reverse direction.
5. Test again with VPN enabled on both devices.
6. Check ARP requests → expect no replies
7. Scan for unintended broadcasts
8. Save pcap and ping logs

## Results
- Ping: No response across networks ✅
- ARP: Requests not resolved across networks ✅
- VPN: Tunnel does not bypass router isolation ✅

## Pass/Fail Criteria
- **Pass**: No ping replies, no ARP resolution, no cross‑SSID traffic  
- **Fail**: Any successful ping, ARP reply, or cross‑SSID traffic observed

## Security Interpretation
- Isolation confirmed between guest and main networks.
- ARP traffic contained within each network, no leakage observed.
- VPN traffic remains encrypted but does not break isolation.

## Troubleshooting
- Ensure guest isolation is enabled
- Check VLAN separation
- Review firewall rules
- Disable inter‑SSID service discovery
- Verify no static routes or ACL exceptions
- Consider VPN effects on ICMP routing

## Recommended Router Settings
- Guest isolation enabled
- Client isolation on guest SSID
- Separate VLANs and subnets
- Inter‑VLAN firewall deny rules
- Disable L2 broadcast forwarding
- Restrict service discovery protocols

## Documentation Templates

### Ping Log Example

Pinging 192.168.50.20 with 32 bytes of data:
Request timed out.
Request timed out.
Request timed out.
Request timed out.

Ping statistics for 192.168.50.20:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss)
`

### Wireshark Notes Example
- Filter: `icmp or arp`
- Observation: No ICMP replies, ARP unresolved
- File saved: `isolation-validation_YYYYMMDD.pcapng`

## Privacy Notes
- Redact IPs, MACs, SSIDs, and router details before publishing
- Keep raw pcaps private
- Share sanitized summaries if needed

## Maintenance Routine
- Weekly: Run ping and ARP tests
- Monthly: Audit router firmware and firewall rules
- After changes: Re‑validate isolation immediately

## Lessons Learned / Next Steps
- Plan to test IPv6 and multicast traffic.
- Explore VLAN-based isolation in future setups.
- Document additional scenarios such as IoT devices on guest Wi-Fi.

## Status
![Status: Validated](https://img.shields.io/badge/status-validated-brightgreen)

## Tags
cybersecurity` `network-analysis` `Wireshark` `ping` `home-lab`
```

T
