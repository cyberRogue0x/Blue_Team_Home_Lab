
| Protocol | Why we query it	What to look out for |
|--|--|
| DNS	| See which domains a computer tries to contact.	Suspicious domains, repeated failed lookups, unusual domain names, excessive queries. |
| TCP	| Investigate connections between computers and servers.	Repeated connection attempts, unanswered SYN packets, unusual ports, unexpected destinations. |
| UDP	| Investigate fast, connectionless traffic such as DNS.	Unusual traffic volume, repeated requests, unexpected destinations or ports. |
| HTTP | Examine unencrypted web requests and responses.	Suspicious URLs, unusual HTTP methods, unexpected file downloads, error responses. |
| TLS/HTTPS	| Examine metadata about encrypted connections.	Unexpected server names, unusual destinations, suspicious connection patterns. Encrypted content is generally not readable directly. |
| ICMP | Investigate network reachability and diagnostic traffic.	Unusual ping volume, repeated probes, unexpected ICMP traffic. |
| ARP	| See how devices map local IP addresses to MAC addresses.	Repeated conflicting IP-to-MAC mappings, possible ARP spoofing. |
