
# App Architecture Patterns – Internal, Cloud, Hybrid, Public

A reference table for devs, architects, and founders comparing modern app deployment architectures—on-prem, cloud, hybrid, and edge (IoT)—across employee and customer use cases.

> **Note:** This chart is not exhaustive! It’s meant as a starting blueprint or architectural outline. Real-world system design always requires deeper analysis of security, compliance, scalability, data sovereignty, latency, user experience, regulatory requirements, cost, and more. Use this as a launchpad, not a replacement for a full architecture review.

---

## Summary Table: App Architecture Patterns

| **Scenario** | **Server Location** | **User Location** | **User Type** | **Frontend Location** | **Backend Location** | **Access Method** | **Network Setup** |
|--------------|---------------------|-------------------|---------------|----------------------|---------------------|-------------------|-------------------|
| 1. Internal Tool (Employee, On-Prem) | On-prem | Internal LAN | Employee/Admin | Laptop / PC / Tablet / Mobile / IoT Device (LAN) | Data Center (LAN) | VPN, SSO, Direct IP, (plus HTTP(S)/API, SSH, RDP, etc.) | Internal DNS, firewall rules |
| 2. Internal Tool (Employee, Cloud) | Cloud (e.g. AWS) | Remote/Office | Employee/Admin | Laptop / PC / Tablet / Mobile / IoT Device (Internet) | Cloud VM/Service | VPN, SSO, Zero Trust Proxy (all protocols: HTTP(S)/API, SSH, RDP, etc.) | VPN, SSO/OAuth, IP allowlist |
| 3. Public App (Customer, Cloud) | Cloud (e.g. Azure) | Internet | Customer | Browser / Mobile / Desktop App / IoT Device (Internet) | Cloud service (scalable) | HTTPS/Web, OAuth (SSO for customers), API key | DNS, CDN, public APIs |
| 4. Public App (Customer, On-Prem) | On-prem | Internet | Customer | Browser / Mobile / Desktop App / IoT Device (Internet) | DMZ or on-prem server | HTTPS/Web, reverse proxy, WAF (plus optional VPN/SSH for privileged ops) | Public IP, firewall, reverse proxy |
| 5. Hybrid (Employees + Customers, Cloud) | Cloud | Anywhere | Both | Browser / Mobile / Desktop App / IoT Device (Internet) | Cloud (multi-tier) | Multi-tenant, RBAC, SSO (employees), OAuth (customers), HTTP(S)/API | Segmented VPC, WAF, IAM |
| 6. Hybrid (On-prem backend, Cloud frontend) | On-prem + Cloud | Anywhere | Both | Cloud-hosted FE, users | On-prem backend | API Gateway, tunneling, hybrid VPN (plus HTTP(S)/API, SSH, etc.) | Hybrid cloud, private link |

---

## Architecture Breakdown & Visuals

### 1. On-Prem, Internal Employees Only

**Use case:** Legacy business apps, dashboards, file servers, internal AI tools

- **Frontend:** Deployed on Laptop / PC / Tablet / Mobile / IoT Device (LAN), browser or Electron desktop app
- **Backend:** On-prem server, only accessible via internal LAN
- **Access:** VPN, SSO, Direct IP, (plus HTTP(S)/API, SSH, RDP, etc.)
- **Security:** Firewall, no public exposure



[Employee Device (Laptop/PC/Tablet/Mobile/IoT)]
<--LAN-->
[On-Prem Backend Server]


---

### 2. Cloud Server, Remote Employees (Internal App)

**Use case:** Remote work, global teams, company portals

- **Frontend:** Laptop / PC / Tablet / Mobile / IoT Device (Internet), often web app via CI/CD to S3/Blob or Vercel/Netlify
- **Backend:** Cloud (EC2, Azure App Service, etc.)
- **Access:** VPN, SSO, Zero Trust Proxy (all protocols: HTTP(S)/API, SSH, RDP, etc.)
- **Security:** IP allowlist, role-based access



[Remote Employee Device (Laptop/PC/Tablet/Mobile/IoT)]
<--VPN/SSO-->
[Cloud App Server]


---

### 3. Cloud Public App (Customer-Facing)

**Use case:** SaaS, mobile apps, AI chatbots, product dashboards

- **Frontend:** Browser / Mobile / Desktop App / IoT Device (Internet), static site (S3, Azure Blob, Vercel, Netlify) or dynamic SPA (Vue, React)
- **Backend:** Cloud, API gateway in front of services
- **Access:** HTTPS/Web, OAuth (SSO for customers), API key
- **Security:** WAF, DDoS, multi-tenant RBAC



[Customer Device (Browser/Mobile/Desktop App/IoT)]
<--Internet-->
[CDN/Frontend]
<--API-->
[Cloud Backend/LLM]


---

### 4. On-Prem Public App (Rare, Usually DMZ)

**Use case:** Regulated, legacy industries (healthcare, gov’t, SCADA)

- **Frontend:** Browser / Mobile / Desktop App / IoT Device (Internet), public web (reverse proxy or DMZ)
- **Backend:** On-prem in DMZ, with limited public IP exposure
- **Access:** HTTPS/Web, reverse proxy, WAF (plus optional VPN/SSH for privileged ops)
- **Security:** WAF, heavy firewalling, reverse proxies



[Customer Device (Browser/Mobile/Desktop App/IoT)]
<--Internet-->
[Reverse Proxy/DMZ]
<--->
[On-Prem Backend]


---

### 5. Hybrid (Cloud Frontend, On-Prem Backend)

**Use case:** When data must stay on-prem, but frontend is cloud hosted (common with LLM or sensitive apps)

- **Frontend:** Cloud-hosted FE, users on Browser / Mobile / Desktop App / IoT Device (Internet)
- **Backend:** On-prem (private APIs, private link/VPN)
- **Access:** API Gateway, tunneling, hybrid VPN (plus HTTP(S)/API, SSH, etc.)
- **Security:** Hybrid firewall, zero trust, logging



[User Device (Browser/Mobile/Desktop App/IoT)]
<--Internet-->
[Cloud Frontend]
<--API Tunnel-->
[On-Prem Backend]


---

### 6. Hybrid (Cloud Backend, Multiple User Types)

**Use case:** Big SaaS, both internal dashboards and customer portals

- **Frontend:** Multi-tenant, public or private routes (Vue, React) on Browser / Mobile / Desktop App / IoT Device (Internet)
- **Backend:** Cloud, can split into microservices for users vs admins
- **Access:** Multi-tenant, RBAC, SSO (employees), OAuth (customers), HTTP(S)/API



[Employee/Customer Device (Browser/Mobile/Desktop App/IoT)]
<--Internet-->
[Cloud Frontend]
<--API-->
[Cloud Backend]


---

## Glossary & Footnotes

- **SSO (Single Sign-On):** Allows employees or users to log in once and access multiple apps or services. Typically implemented with Okta, Azure AD, etc. for internal users.
- **OAuth:** Authorization framework used for granting third-party apps access to user data. Powers “Sign in with Google/Apple” buttons (acts as SSO for customers in public apps).
- **VPN (Virtual Private Network):** Creates a secure, encrypted connection (tunnel) between user devices and internal networks, allowing remote access to on-prem or cloud apps as if you were physically on the internal network.
- **Zero Trust Proxy:** Security approach where every request—internal or external—must be authenticated and authorized, regardless of the user’s network location.
- **API Gateway:** Service that manages, secures, and routes API traffic between frontend clients and backend services. Can handle authentication, rate limiting, logging, etc.
- **WAF (Web Application Firewall):** Filters and monitors HTTP(S) traffic to block malicious requests and attacks against web applications.
- **RBAC (Role-Based Access Control):** Security approach that restricts system access to users based on their role within the organization.
- **DMZ (Demilitarized Zone):** Isolated network segment that adds a buffer between an organization’s internal network and the public internet, often used for hosting public-facing services securely.
- **CDN (Content Delivery Network):** Distributes frontend/static content globally to reduce latency and improve app performance for users everywhere.

**Footnote:**  
*All connections—whether internal or external—ultimately use HTTP(S), SSH, RDP, or similar protocols. VPNs, SSO, OAuth, and Zero Trust are additional layers that secure or control these underlying connections, but the data still rides the same rails!*

---
