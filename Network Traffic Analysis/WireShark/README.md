# Network Traffic Analysis Using Wireshark

A beginner-friendly network traffic analysis project documenting a bounded packet capture review. It shows how protocol-focused Wireshark display filters can help an analyst understand DNS lookups, TCP connection setup, and TLS connection metadata.

> **Evidence note:** The screenshots uploaded in the referenced ChatGPT conversation were not available as files in the conversation record or project workspace. No screenshots have been fabricated. See [`evidence/README.md`](evidence/README.md) for the five prioritized images to add.

## Project summary

- **Tool:** Wireshark
- **Focus:** DNS, TCP, and TLS metadata
- **Packets captured:** 3,511
- **DNS query view:** 60 displayed packets with `dns.flags.response == 0`
- **Assessment:** The reviewed traffic was consistent with normal browsing and application activity. No malicious activity was confirmed in this limited capture.

## Findings at a glance

| Area | Observed | Why it matters |
|---|---|---|
| Capture overview | 3,511 packets | Establishes the size of the capture under review. |
| Host and resolver | Host `10.176.15.20`; DNS resolver `10.176.15.139` | Identifies the internal endpoints visible in the capture. |
| DNS | 60 query packets displayed; domains included Spotify, Google, ChatGPT, and Cloudflare | Shows name-resolution activity associated with applications and browsing. |
| DNS errors | `dns.flags.rcode != 0` returned no displayed packets | No DNS response errors matched this filter in the capture. |
| TCP | A TCP handshake was inspected | Shows connection establishment; the reviewed handshake alone does not establish application intent. |
| TLS | Client Hello SNI `spclient.wg.spotify.com` | Reveals a requested server name in visible TLS handshake metadata. |

See [`investigation-report.md`](investigation-report.md) for scope, method, interpretation, and limitations and [`findings/observations.md`](findings/observations.md) for protocol notes and filters.

## Display filters used
| Filters | What they do|
|--|--|
| dns.flags.response == 0 | DNS queries (requests). It displayed 60 packets in this capture. |
| dns.flags.rcode != 0 | DNS responses with a nonzero response code, which can indicate lookup errors. No packets matched in this capture. |
| tcp.flags.syn == 1 && tcp.flags.ack == 0 | Initial TCP SYN packets, used to find connection attempts. Follow each flow to inspect the SYN/ACK and final ACK of the handshake. |
| tls.handshake.type == 1 | TLS Client Hello messages, which can expose handshake metadata such as the requested server name (SNI). 

The TCP filter isolates initial SYN packets; inspect the corresponding SYN/ACK and final ACK to review a full three-way handshake. The TLS filter shows Client Hello messages when dissected as TLS. Filters narrow the displayed packets; they do not alter the capture.

## Skills Gained
- Wireshark display filtering
- DNS query and response analysis
- TCP handshake analysis
- TLS Client Hello and SNI inspection
- Packet capture interpretation
- Investigation documentation
- Evidence handling and privacy awareness


## Privacy and handling

Redact internal IP addresses, usernames, hostnames, or browsing history if they are private. This repository intentionally excludes the raw PCAP; only share a capture after reviewing it for sensitive information and obtaining the appropriate authorization.

