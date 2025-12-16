
# Validating Reverse Isolation Between Devices Using Wireshark and ICMP Ping

## Objective
Confirm bidirectional network isolation between a laptop and a mobile device connected to separate Wi‑Fi networks (main and guest) by analyzing ICMP traffic and ARP behavior in Wireshark.

## Environment Setup
- Laptop: Connected to main Wi‑Fi (Example IP: 192.168.1.10)
- Mobile: Connected to guest Wi‑Fi (Example IP: 192.168.50.20)
- Router: Guest isolation enabled
- Tools: Wireshark (latest stable), OS-native ping utilities

## Prerequisites
- Admin access to router settings
- Basic knowledge of Wireshark filters
- Device IPs and gateways identified
- Time synchronization optional but useful

## Network Assumptions
- Main and guest SSIDs use different subnets (e.g., 192.168.1.0/24 vs 192.168.50.0/24)
- Guest SSID mapped to isolated VLAN
- No broadcast bridging between SSIDs

## Checklist
1. Verify SSID separation
2. Confirm IP ranges differ
3. Enable guest/client isolation
4. Start Wireshark capture
5. Run bidirectional ping tests
6. Validate ARP behavior
7. Save results

## Wireshark Filters
- ICMP only: `icmp`
- ARP only: `arp`
- ICMP between specific IPs:  
  `icmp && (ip.src == 192.168.1.10 && ip.dst == 192.168.50.20) || (ip.src == 192.168.50.20 && ip.dst == 192.168.1.10)`
- ICMP or ARP: `icmp or arp`

## Validation Steps
1. Record baseline IPs and gateways
2. Start Wireshark on laptop Wi‑Fi interface
3. Ping mobile from laptop → expect timeout
4. Ping laptop from mobile → expect timeout
5. Check ARP requests → expect no replies
6. Scan for unintended broadcasts
7. Save pcap and ping logs

## Pass/Fail Criteria
- Pass: No ping replies, no ARP resolution, no cross‑SSID traffic
- Fail: Any successful ping, ARP reply, or cross‑SSID traffic observed

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
```
Pinging 192.168.50.20 with 32 bytes of data:
Request timed out.
Request timed out.
Request timed out.
Request timed out.

Ping statistics for 192.168.50.20:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss)
```

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
```


