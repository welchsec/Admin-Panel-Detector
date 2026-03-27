# 🔐 WelchSec Admin Panel Detector

A free, browser-based tool for security professionals and system administrators to identify exposed admin panels and management interfaces on their own infrastructure. Helps organizations find and remediate publicly accessible login panels before attackers do.

**Live Tool → [welchsec.github.io/Admin-Panel-Detector](https://welchsec.github.io/Admin-Panel-Detector/)**

---

## ⚠️ Legal Disclaimer

> **Only use this tool against infrastructure you own or have explicit written permission to test.**
> Unauthorized scanning of systems you do not own may violate computer fraud laws including the Computer Fraud and Abuse Act (CFAA) and equivalent legislation in your jurisdiction.
> This tool is designed exclusively for authorized security auditing, attack surface management, and defensive hardening of your own systems.

---

## 🧰 What It Does

The tool has two modes accessible from the top navigation bar:

---

### 🔍 Mode 1 — Panel Probe

Directly probes a target domain or IP address for 100+ known admin panel paths using your Cloudflare Worker proxy. Each path is tested and classified by response.

**Result classifications:**

| Status | HTTP Code | Meaning | Action Required |
|--------|-----------|---------|----------------|
| **EXPOSED** | 200 | Panel is publicly accessible | Immediate remediation required |
| **REDIRECT** | 3xx | Redirects to a login page | Review and restrict access |
| **POSSIBLE** | 401 / 403 | Panel exists but requires authentication | Verify access controls are sufficient |
| **NOT FOUND** | 404 | Path does not exist | No action needed |

**Features:**
- Protocol selection — HTTPS only, HTTP only, or try both
- Toggle categories on/off to narrow your scan
- Live progress bar and running stats
- Filter results by status (Exposed / Redirect / Possible)
- CSV export of all findings
- Concurrency-safe — yields to the browser to prevent timeouts

---

### 🔎 Mode 2 — Google Dorks

Generates targeted Google search queries scoped to your domain to find admin panels that have already been indexed by search engines. If Google can find them, so can attackers.

**Features:**
- 48+ targeted dork queries across 8 categories
- Direct **GOOGLE** links — click to open each query in Google Search
- **COPY** button for each query
- **COPY ALL** to export all queries at once
- Flat list view for easy sharing with your security team
- Expandable/collapsible category sections

> **No results in Google = good.** Results found means your admin panel is publicly indexed and needs to be removed or restricted immediately.

---

## 📋 Coverage — 100+ Panels Across 8 Categories

### 🌐 Network Devices
Routers, switches, firewalls and network appliances including Cisco ASA, pfSense, MikroTik, Ubiquiti UniFi, Fortinet FortiGate, SonicWall, Palo Alto, CheckPoint, Juniper SRX, Netgear, TP-Link, D-Link and Linksys.

### 🖥 Web Servers & Hosting Panels
cPanel, WHM, Plesk, DirectAdmin, Webmin, Virtualmin, Apache Server Status, Apache Server Info, Nginx Status, IIS Manager, and phpinfo() disclosure pages.

### 📝 CMS Platforms
WordPress admin and login pages, Joomla administrator panel, Drupal admin, Magento admin, Django admin interface, Ghost CMS, Laravel Telescope, and TYPO3 backend.

### 🔒 Remote Access
Citrix Gateway, Pulse Secure VPN, GlobalProtect VPN, OpenVPN Access Server, Outlook Web App (OWA), Remote Desktop Web Gateway (RDWeb), VMware Horizon, Apache Guacamole, NoMachine, and TeamViewer.

### ⚙ DevOps & CI/CD
Jenkins CI, GitLab, Gitea, Gogs, Portainer Docker management, Kubernetes Dashboard, Rancher, Grafana, Kibana, Prometheus, SonarQube, Nexus Repository, Jira, and Confluence.

### 🗄 Databases
phpMyAdmin, Adminer, MongoDB Express, Redis Commander, Elasticsearch, CouchDB Fauxton, pgAdmin, and InfluxDB.

### 🛡 Security Tools
Splunk SIEM, Nessus vulnerability scanner, OpenVAS, Graylog, Wazuh, Security Onion, OSSEC, Metasploit Pro, Burp Suite, and Qualys.

### 📷 IoT & Cameras
Hikvision, Dahua, Axis, Amcrest, Reolink and Foscam IP cameras, generic DVR/NVR systems, Building Management Systems (BMS), BACnet interfaces, SCADA HMI panels, and network printers.

---

## 🚀 Setup Guide

### Step 1 — Deploy the Cloudflare Worker

The Panel Probe mode requires a Cloudflare Worker to make requests on your behalf. This is necessary because browsers block direct cross-origin requests from static GitHub Pages sites.

1. Go to [workers.cloudflare.com](https://workers.cloudflare.com) and sign up — no credit card needed

2. Click **Workers & Pages → Create Application → Create Worker**

3. Name it something like `welchsec-proxy` and click **Deploy**

4. Click **Edit Code**, select all the default code and delete it

5. Copy the contents of `cloudflare-worker.js` from the [WelchSec Recon Tool repo](https://github.com/welchsec/Recon-Tool) and paste it in

6. Click **Deploy**

7. Copy your Worker URL:
   ```
   https://welchsec-proxy.yourname.workers.dev
   ```

> **Already have a Worker from another WelchSec tool?**
> The same worker handles everything — just paste your existing Worker URL. No changes needed.

---

### Step 2 — Use the Tool

**Panel Probe:**
1. Open [welchsec.github.io/Admin-Panel-Detector](https://welchsec.github.io/Admin-Panel-Detector/)
2. Enter your target domain or IP address
3. Paste your Cloudflare Worker URL
4. Select the categories relevant to your infrastructure
5. Click **▶ PROBE PANELS**

**Google Dorks:**
1. Switch to the **GOOGLE DORKS** tab
2. Enter your target domain
3. Select categories
4. Click **▶ GENERATE DORKS**
5. Click **GOOGLE** next to any query to open it in Google Search

No API keys required.

---

## 💡 Usage Tips

### Recommended Workflow
1. Start with **Google Dorks** — check what Google has already indexed. This is passive and leaves no footprint on your target systems.
2. Run **Panel Probe** against your own infrastructure to find panels that may not be indexed but are still publicly accessible.
3. **Export to CSV** and include findings in your security audit report.
4. For each exposed panel found, follow the remediation steps below.

### Interpreting Results
- **EXPOSED (HTTP 200)** panels are the highest priority — they are publicly accessible and potentially accessible without credentials if default passwords haven't been changed
- **POSSIBLE (HTTP 401/403)** results mean the panel exists and is responding — verify that authentication is enforced and that the panel is not accessible from the public internet unless required
- **REDIRECT (HTTP 3xx)** results are worth investigating — follow the redirect to understand where it leads
- **NOT FOUND** does not guarantee a panel doesn't exist — it may be on a non-standard port or path

### Remediation Steps for Exposed Panels
1. **Restrict by IP** — whitelist only known administrator IP addresses using firewall rules or `.htaccess`
2. **Move to internal network** — admin interfaces should not be accessible from the public internet
3. **Change default credentials** — immediately change any default usernames and passwords
4. **Enable MFA** — add multi-factor authentication to all admin interfaces
5. **Remove Google indexing** — use Google Search Console to request removal of indexed admin pages
6. **Add to robots.txt** — add disallow rules to prevent future indexing (note: this is not a security control on its own)

---

## 💰 Cost

| Component | Cost |
|-----------|------|
| GitHub Pages hosting | Free |
| Cloudflare Worker | Free (100k requests/day) |

---

## 🔧 Troubleshooting

**Probe returns no results at all**
Check that your Cloudflare Worker URL is correct and the worker has been deployed. Open browser DevTools (F12) → Console for specific error messages.

**Everything shows NOT FOUND**
The target may be behind a firewall that drops requests rather than returning 404. Try a known path first (e.g. `/wp-admin/` on a known WordPress site) to confirm the probe is working.

**Google Dork returns no results**
No results is a good outcome — it means Google has not indexed those pages. If you expect results and see none, the panel may exist but not be indexed, or Google may have recently de-indexed it.

**GitHub Pages shows the README instead of the tool**
The HTML file must be named exactly `index.html`. Rename it in your GitHub repo if needed.

---

## 🔗 Related Tools

| Tool | Description | Link |
|------|-------------|------|
| Recon Suite | 9-module recon toolkit for bug bounty | [welchsec.github.io/Recon-Tool](https://welchsec.github.io/Recon-Tool/) |
| Phishing Analyzer | URL analysis for phishing indicators | [welchsec.github.io/Phishing-Analyzer](https://welchsec.github.io/Phishing-Analyzer/) |
| OSINT Dashboard | Domain intelligence aggregator | [welchsec.github.io/OSINT-Dashboard](https://welchsec.github.io/OSINT-Dashboard/) |
| C2 Domain Monitor | Monitor for C2 infrastructure and malicious domains | [welchsec.github.io/C2-Monitor](https://welchsec.github.io/C2-Monitor/) |
| Intrusion Report | DFIR-style reports from live threat intel | [welchsec.github.io/Intrusion-Report](https://welchsec.github.io/Intrusion-Report/) |
| TTP Report | AI-powered MITRE ATT&CK threat intelligence | [welchsec.github.io/TTP-Threat-Report](https://welchsec.github.io/TTP-Threat-Report/) |

All tools share the same Cloudflare Worker — one deployment handles everything.

---

## 📬 Contact

Built by [@welchsec](https://github.com/welchsec)
