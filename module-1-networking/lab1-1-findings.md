# Lab 1.1 — Networking Command Toolkit

## VM Details

- Provider: AWS
- Region: ap-south-1 (Mumbai)
- OS: Ubuntu
- Instance: t3.micro
## Ping Test

Command:

```bash
ping -c 5 8.8.8.8

```
Result:
- Packets transmitted: 5
- Packets received: 5
- Packet loss: 0%
- Average RTT: 1.399 ms
- TTL: 117

## Traceroute Test

Command:

```bash
traceroute google.com
```

Result:
- Total hops: 3
- `* * *` hops: None
- Destination reached at hop 3
## DNS Investigation

### Default DNS

Command:

```bash
dig google.com
```

Result:
- Status: NOERROR
- DNS server: 127.0.0.53
- Query time: 1 ms
- Answers: 6 IPv4 addresses
### DNS Comparison with 1.1.1.1

Command:

```bash
dig google.com @1.1.1.1
```
Result:
- DNS server: 1.1.1.1
- Query time: 2 ms
- Answers: 6 IPv4 addresses
- The returned IP addresses were different from the default DNS response.

## Port Scanning

Command:

```bash
nmap -sV localhost
```

Result:
- Host: localhost (127.0.0.1)
- Open port: 22/tcp
- Service: SSH
- Version: OpenSSH 10.2p1 Ubuntu 2ubuntu3.2
- Closed TCP ports: 999
## TLS Connection Test

Command:

```bash
curl -v https://www.google.com 2>&1 | head -40
```

Result:
- TLS version: TLS 1.3
- Cipher: TLS_AES_256_GCM_SHA384
- Subject: CN=www.google.com
- Issuer: Google Trust Services, CN=WR2
- Certificate expiry: Nov 27 2026
## TCP Port 443 Traceroute

Command:

```bash
sudo traceroute -T -p 443 google.com

```
Result:
- Total hops: 3
- Hop 1: Responded
- Hop 2: * * * (no response)
- Hop 3: Reached Google destination
- Destination port: 443
## Observations

- TTL observed: 117
- TTL explanation: TTL limits the lifetime of an IP packet and is reduced as the packet passes through routers.
- Traceroute number of hops: 3
- Any `* * *` hops in the normal traceroute: No
- Destination reached at hop: 3
## DNS Comparison

### Default DNS

Command:

```bash
nslookup google.com
```

Result:
- DNS server: 127.0.0.53
- IPv4 address: 142.250.195.46
- IPv6 addresses: returned

### Cloudflare DNS

Command:

```bash
nslookup google.com 1.1.1.1
```

Result:
- DNS server: 1.1.1.1
- IPv4 address: 142.251.42.238
- IPv6 addresses: returned
- The DNS responses returned different Google IP addresses.
## SSL Certificate Inspection

Command:

```bash
openssl s_client -connect google.com:443 </dev/null | openssl x509 -noout -text | head -40
```

Result:
- Subject: CN=*.google.com
- Issuer: Google Trust Services, CN=WR2
- Valid from: Sep 4, 2026
- Valid until: Nov 27, 2026
- Public key: EC / P-256
- Extended Key Usage: TLS Web Server Authentication
