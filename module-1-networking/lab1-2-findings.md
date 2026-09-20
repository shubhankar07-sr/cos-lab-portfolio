# Lab 1.2 — Web Server + TLS Certificate

## VM Details

- Provider: AWS
- Region: ap-south-1 (Mumbai)
- OS: Ubuntu
- Instance: t3.micro
- Public IP: 16.4.23.10

## Nginx Web Server

Nginx was installed successfully.

Command:

```bash
sudo apt install -y nginx
```

## Firewall Configuration

UFW was enabled with the following allowed ports:

- SSH: 22/tcp
- HTTP: 80/tcp
- HTTPS: 443/tcp

Firewall status:

```text
Status: active
22/tcp  ALLOW
80/tcp  ALLOW
443/tcp ALLOW
```

## Self-Signed TLS Certificate

A self-signed certificate was generated for the VM public IP.

Certificate subject:

- CN: 16.4.23.10

Certificate validity:

- Start: Sep 20, 2026
- Expiry: Sep 20, 2027

Certificate algorithms:

- Public Key Algorithm: RSA
- Public Key Size: 2048 bit
- Signature Algorithm: sha256WithRSAEncryption

Issuer:

- C=IN, ST=Karnataka, L=Bangalore, O=InstaSafe Lab, CN=16.4.23.10

Subject:

- C=IN, ST=Karnataka, L=Bangalore, O=InstaSafe Lab, CN=16.4.23.10

The issuer and subject are the same because this is a self-signed certificate.

## Nginx HTTPS Configuration

Nginx was configured to:

- Listen on HTTPS port 443
- Use the self-signed certificate
- Support TLS 1.2 and TLS 1.3
- Redirect HTTP port 80 traffic to HTTPS

Nginx configuration test:

```text
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

## HTTPS Connection Test

From the Windows laptop, the HTTPS port was tested successfully.

Command:

```powershell
Test-NetConnection 16.4.23.10 -Port 443
```

Result:

```text
TcpTestSucceeded : True
```

## HTTPS with Certificate Verification Disabled

Command:

```powershell
curl.exe -k https://16.4.23.10
```

Result:

```html
<h1>COGS Lab — TLS Demo</h1><p>Hello from InstaSafe Support Lab!</p>
```

This confirms that the Nginx HTTPS server is working and serving the configured web page.

## HTTPS without -k

Command:

```powershell
curl.exe https://16.4.23.10
```

Result:

```text
SEC_E_UNTRUSTED_ROOT
The certificate chain was issued by an authority that is not trusted.
```

## Why does curl without -k fail?

The certificate is self-signed. It was not issued by a Certificate Authority trusted by the Windows operating system.

The `-k` option tells curl to skip normal certificate verification, so the HTTPS connection can still be tested.

For a production environment, the certificate should be issued by a trusted Certificate Authority, or the self-signed CA/certificate should be explicitly installed and trusted on the client system.

## OpenSSL Certificate Inspection

The certificate was inspected from the Windows laptop using OpenSSL.

Important findings:

- Subject: CN=16.4.23.10
- Issuer: CN=16.4.23.10
- Validity: Sep 20, 2026 to Sep 20, 2027
- Public Key Algorithm: RSA
- Public Key Size: 2048 bit
- Signature Algorithm: sha256WithRSAEncryption
- Certificate Type: Self-signed

## Summary

Lab 1.2 demonstrated:

- Nginx web server installation
- UFW firewall configuration
- HTTPS configuration using Nginx
- Creation of a self-signed TLS certificate
- TLS certificate inspection
- Successful HTTPS connection testing
- The difference between certificate verification with and without `-k`
