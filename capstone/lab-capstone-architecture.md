# Capstone — Mini ZTNA Architecture

## 1. Overview

This capstone demonstrates a small ZTNA-like architecture using the cloud lab components built during the previous modules.

The architecture connects a Windows laptop to a WireGuard tunnel hosted on VM2. Nginx on VM2 acts as the gateway/reverse proxy and forwards `/app` requests to Keycloak running on VM1.

## 2. Architecture

```text
                    Windows Laptop
                         |
                         | WireGuard
                         | 10.0.0.2
                         v
                +----------------------+
                | VM2 - Gateway        |
                | 10.0.0.1             |
                | WireGuard + Nginx    |
                +----------+-----------+
                           |
                           | Nginx reverse proxy
                           | /app
                           v
                +----------------------+
                | VM1 - Identity/App   |
                | 172.31.13.177        |
                | Keycloak :8080       |
                +----------------------+

Monitoring:
Uptime Kuma + Prometheus monitor the lab infrastructure.

## 3. Components

### Windows Laptop

The Windows laptop acts as the client device.

The laptop connects to VM2 using WireGuard and receives the tunnel address:

```text
10.0.0.2

VM2 — Gateway
VM2 acts as the gateway for the architecture.
Private IP:
172.31.9.86

WireGuard tunnel IP:
10.0.0.1

Services running on VM2 include:
- WireGuard
- Nginx
- Uptime Kuma
- Prometheus
- Grafana
VM1 — Identity/Application Server
VM1 hosts Keycloak.
Private IP:
172.31.13.177

Keycloak:
http://172.31.13.177:8080

Keycloak was already configured during the Identity Management labs.
Monitoring
The lab uses:
- Uptime Kuma for availability monitoring
- Prometheus for infrastructure metrics
- Grafana for monitoring dashboards

---

## 4. Architecture Setup

### 4.1 WireGuard Tunnel

WireGuard was configured on VM2 as the VPN server.

The server uses:

```text
Tunnel IP: 10.0.0.1
Port: 51820/UDP

The Windows laptop was configured as the WireGuard client:
Client IP: 10.0.0.2

The WireGuard tunnel was verified successfully.
The client was able to communicate with the gateway using the tunnel address.
###4.2 Nginx Reverse Proxy
Nginx was configured on VM2 as the gateway/reverse proxy.
The /app location forwards requests to Keycloak on VM1.
Configuration:
server {
    listen 80;
    server_name 10.0.0.1;

    location /app {
        proxy_pass http://172.31.13.177:8080/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}

The Nginx configuration was validated using:
sudo nginx -t

The configuration test completed successfully and Nginx was reloaded.
###4.3 Keycloak
Keycloak is running on VM1 using Docker.
The Keycloak service is available on port:
8080

The capstone uses the existing Keycloak deployment from Lab 2.2.
The gateway forwards /app requests from VM2 to the Keycloak service on VM1.
###4.4 Monitoring
Uptime Kuma and Prometheus were already deployed during the monitoring labs.
They provide availability and infrastructure monitoring for the lab environment.
Uptime Kuma can be used to monitor gateway and application availability, while Prometheus collects system metrics for analysis through Grafana.

---

## 5. End-to-End Request Flow

The complete request flow is:

```text
Windows Laptop
      |
      | WireGuard tunnel
      | 10.0.0.2 → 10.0.0.1
      v
VM2 - Nginx Gateway
      |
      | HTTP /app
      | Reverse proxy
      v
VM1 - Keycloak
      |
      v
Keycloak application

When the client sends:
http://10.0.0.1/app

the request travels through the WireGuard tunnel to VM2.
Nginx receives the request on VM2 and forwards the /app request to Keycloak running on VM1.

---

## 6. WireGuard Verification

The WireGuard tunnel was verified from the Windows laptop.

The gateway tunnel address was:

```text
10.0.0.1
The client tunnel address was:
10.0.0.2

Connectivity was tested using:
ping 10.0.0.1

The ping test successfully returned replies with:
0% loss

This confirmed that the Windows client could communicate with the WireGuard gateway through the tunnel.

---

## 7. End-to-End Application Verification

After establishing the WireGuard tunnel, the application path was tested from the Windows laptop.

The command used was:

```powershell
curl.exe http://10.0.0.1/app

The request successfully returned the Keycloak Welcome page.
The response contained:
Welcome to Keycloak

This verified the complete network path:
Windows Laptop
      ↓
WireGuard
      ↓
VM2 / Nginx
      ↓
VM1 / Keycloak

Evidence
 
The screenshot shows the WireGuard connectivity test and the successful Keycloak response through the Nginx gateway.
8. Firewall Verification
The WireGuard service uses:
UDP 51820

The AWS security group was configured to allow WireGuard traffic on UDP port 51820 from the client network.
UFW on VM2 was also configured to allow:
51820/udp

After re-enabling UFW, the WireGuard tunnel remained functional.
The following tests continued to work:
ping 10.0.0.1

and:
curl.exe http://10.0.0.1/app

This confirmed that the firewall configuration did not prevent the established WireGuard-based application flow.
9. Monitoring
The cloud lab already contains monitoring infrastructure using Uptime Kuma, Prometheus, and Grafana.
Uptime Kuma provides availability monitoring and can detect when a service becomes unavailable.
Prometheus collects infrastructure metrics such as CPU, memory, and network information.
Grafana provides dashboards for visualizing the collected metrics.
The monitoring architecture can therefore be represented as:
             +----------------+
             |  VM2 Gateway   |
             +-------+--------+
                     |
             +-------+--------+
             |                |
             v                v
       Uptime Kuma       Prometheus
                              |
                              v
                           Grafana

The monitoring components provide visibility into the availability and health of the cloud lab infrastructure.
10. Mapping to a Real ZTNA Architecture
The mini architecture demonstrates several concepts that are similar to a real ZTNA deployment.
Lab Component	ZTNA Concept
WireGuard	Secure encrypted tunnel
Nginx Gateway	Application gateway / traffic routing
Keycloak	Identity Provider
Uptime Kuma	Availability monitoring
Prometheus	Infrastructure metrics
Grafana	Monitoring and visualization


WireGuard
WireGuard provides an encrypted connection between the client and the lab gateway.
In a real ZTNA platform, the endpoint agent establishes a secure connection to the access infrastructure.
Nginx
Nginx acts as the gateway and reverse proxy.
In a production ZTNA platform, a gateway or application connector performs application-level traffic routing and access enforcement.
Keycloak
Keycloak represents the identity provider in this lab.
A production ZTNA platform can integrate with identity providers such as Microsoft Entra ID, Okta, or other enterprise identity systems.
Monitoring
Uptime Kuma, Prometheus, and Grafana demonstrate the monitoring side of the architecture.
Production ZTNA platforms require continuous monitoring, logging, alerting, and operational visibility.
11. What Is Missing Compared With Production ZTNA?
This lab is a simplified demonstration and does not implement all the capabilities of a production ZTNA platform.
Important missing capabilities include:
1. Device Posture Checking
A production solution can evaluate the security state of the endpoint before granting access.
Examples include:
- Operating system version
- Antivirus status
- Device compliance
- Security configuration
- Device registration
The lab does not implement full device posture evaluation.
2. Identity-Based Access Policies
Production ZTNA systems normally apply detailed policies based on:
- User identity
- Group membership
- Device identity
- Application
- Location
- Risk
- Authentication status
The lab demonstrates the identity-provider component but does not implement a complete production policy engine.
3. Multi-Factor Authentication Integration
Keycloak can support MFA, and MFA was explored during the earlier identity labs.
However, the capstone flow itself does not demonstrate a complete production MFA-based application access policy.
4. Application-Level Segmentation
Production ZTNA provides application-specific access rather than exposing an entire network.
The lab demonstrates a simplified /app reverse-proxy route.
5. Centralized Logging
Production environments require centralized security and access logs.
The lab does not implement a full centralized SIEM/log-management platform.
6. High Availability
Production ZTNA infrastructure normally requires redundancy and high availability.
This capstone uses individual lab VMs and therefore does not provide production-grade redundancy.
7. Certificate and Key Management
Production deployments require secure lifecycle management for:
- TLS certificates
- Encryption keys
- Identity credentials
- Device certificates
The lab uses a simplified configuration.
12. Reflection — Mini-ZTNA vs Production ZTNA
This capstone helped demonstrate the basic architecture and traffic flow behind a simplified ZTNA-style solution. Instead of allowing a client to directly access a backend application, the client first establishes a secure WireGuard tunnel to a gateway. Nginx on the gateway then acts as a reverse proxy and forwards application requests to the backend service running on another VM.
The main concept I learned from this exercise is that secure application access can be separated into different layers. WireGuard provides the encrypted network path, Nginx provides gateway and reverse-proxy functionality, and Keycloak provides an identity-management component. Uptime Kuma, Prometheus, and Grafana add monitoring and visibility to the environment.
The end-to-end test demonstrated that a request from the Windows laptop could travel through the WireGuard tunnel to the VM2 gateway and then be forwarded by Nginx to Keycloak running on VM1. The command curl.exe http://10.0.0.1/app returned the Keycloak Welcome page, proving that the complete network and proxy path was functioning.
However, the lab is only a simplified representation of a production ZTNA platform. A real enterprise deployment would require more advanced identity-based access policies, device posture assessment, MFA enforcement, application-level segmentation, centralized logging, certificate management, high availability, and detailed auditing.
Another important difference is that the lab does not demonstrate a complete production authorization decision for the application. Keycloak is used as the identity-provider component, but the demonstrated curl test verifies the network and proxy path to the Keycloak application rather than proving a full production ZTNA policy enforcement workflow.
Overall, the capstone connected the concepts learned throughout the cloud lab. Networking, identity, monitoring, gateway routing, and incident/operations practices were combined into one architecture. The exercise also showed how individual cloud components can be combined to create a simplified model of a larger enterprise security architecture.
13. Lessons Learned
The main lessons learned from the capstone are:
1. A VPN tunnel can provide a secure communication path between a client and gateway.
2. WireGuard provides encrypted connectivity using public-key cryptography.
3. Nginx can act as a reverse proxy between a client-facing gateway and backend application.
4. Identity providers such as Keycloak can provide centralized authentication services.
5. Monitoring is important for detecting availability and infrastructure problems.
6. Prometheus and Grafana can provide detailed infrastructure visibility.
7. A production ZTNA solution requires more than network tunneling.
8. Identity, device posture, application policies, monitoring, logging, and auditing are important parts of a complete ZTNA architecture.
9. Testing the complete request path is important when troubleshooting cloud infrastructure.
10. Combining the components from multiple labs makes it easier to understand how cloud networking, identity, monitoring, and operations work together.
14. Final Verification
The following capstone components were verified:
Component	Status
Windows WireGuard Client	Verified
WireGuard Tunnel	Verified
VM2 Gateway	Verified
Nginx Reverse Proxy	Verified
VM1 Keycloak	Verified
/app Proxy Route	Verified
Ping through tunnel	Verified
End-to-End curl	Verified
Keycloak response	Verified
Firewall configuration	Verified
Monitoring infrastructure	Available


Final End-to-End Test
The final test was:
ping 10.0.0.1

followed by:
curl.exe http://10.0.0.1/app

The ping test succeeded with 0% packet loss and the curl request returned the Keycloak Welcome page.
Therefore, the demonstrated lab flow was:
Windows Laptop
      |
      | WireGuard
      v
10.0.0.1 / VM2
      |
      | Nginx /app
      v
172.31.13.177:8080
      |
      v
Keycloak

Conclusion
The capstone successfully combined the networking, identity, gateway, and monitoring concepts developed during the cloud lab into a single mini ZTNA-like architecture.
The implementation demonstrates secure tunneled connectivity, gateway-based application routing, identity-provider integration, and infrastructure monitoring while also highlighting the additional capabilities required for a production-grade ZTNA deployment.

