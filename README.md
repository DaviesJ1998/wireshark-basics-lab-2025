# wireshark-basics-lab-2025
Hands-on Wireshark lab: capture, filter, and analyse HTTP &amp; DNS traffic from a laptop (OPSEC redacted).

# Wireshark Basics Lab — 2025

**Purpose:** Demonstrate basic packet capture and network analysis on a laptop using Wireshark. This lab shows how to capture traffic, apply useful display filters, and inspect HTTP and DNS exchanges. All sensitive addresses are redacted for OPSEC.

**Author:** _Your Name_  
**Lab date:** October 2025  
**Redacted example IP used in screenshots / commands:** `10.0.0.50`

---

## Quick summary

- Created project folder: `~/Wireshark_project`  
- Confirmed local IP with `ip addr show` (redacted in public report)  
- Launched Wireshark, selected wireless interface (`wlo1`) and captured traffic  
- Filtered and inspected HTTP (GET / 200 OK) and DNS (query to 8.8.8.8) packets  
- Learned safe OPSEC practices for publishing captures and screenshots

---

# Key observations & interpretation

**HTTP (what you’ll see in the packet list)**

* `GET /testcat_cv.html` — This is the browser’s request for the page. It indicates the client asked the server for that specific resource.
* `200 OK (text/html)` — The server responded successfully and returned an HTML document. Together, the `GET` and `200 OK` lines show a complete request/response exchange for an unencrypted HTTP resource.

**DNS (what’s happening before the HTTP request)**
When you inspect DNS packets you’ll typically see a query from your laptop to your configured resolver (for example, `8.8.8.8`). A typical line might read:
`Standard query ... AAAA www.testingmcafeesites.com → 8.8.8.8`
This means the laptop asked Google DNS, “What is the IP address for `www.testingmcafeesites.com`?” The resolver replies with the IP, and only then does the browser contact that IP to fetch the HTTP resource.

**Example interpretation (end-to-end)**

1. DNS query → resolver returns an IP address for the hostname.
2. Browser issues `GET /path` to the returned IP.
3. Server responds `200 OK (text/html)` and sends the page content.

---

# Skills demonstrated

* Use the terminal to inspect local network interfaces (`ip addr show`).
* Start Wireshark safely from the terminal and select the correct capture interface.
* Apply display filters to reduce noise and focus analysis (e.g., `ip.addr == 10.0.0.50 && http`, `ip.addr == 10.0.0.50 && dns`).
* Interpret HTTP request/response pairs and DNS query/response behavior in a packet capture.
* Redact private network details for public sharing (basic OPSEC).

---

# OPSEC & safety notes

* Replace real private IPs with a redacted/fake IP (for example, `10.0.0.50`) before publishing screenshots or notes.
* Remove or obfuscate MAC addresses, internal hostnames, user names, or any personally identifying information from screenshots.
* Do **not** publish raw capture files (PCAP/PCAPNG) unless they have been thoroughly sanitized — raw captures can contain sensitive data (IPs, hostnames, cookies, etc.).

---

# Suggested deliverables for your portfolio

* `labs/wireshark-basics.md` — the full lab write-up (already included).
* A few redacted screenshots in `assets/images/` demonstrating the filter bar, packet list, and packet details pane.
* (Optional) A short explanatory video walking through the capture process, filter usage, and packet interpretation.

---

# Suggested screenshots to include

* Wireshark main window showing the filter: `ip.addr == 10.0.0.50 && http` (ensure IP is redacted).
* Packet details pane showing a `GET /...` request and corresponding `200 OK` response.
* DNS query packet showing the destination resolver (e.g., `8.8.8.8`) and the queried hostname.

---

# What I learned

* How DNS and HTTP transactions appear in packet captures and how they relate to one another.
* How to use display filters (`ip.addr`, protocol names like `http` and `dns`) to limit noise and focus on relevant traffic.
* Basic OPSEC practices for publishing capture evidence safely.

---

# Next steps (optional improvements)

* Capture and inspect HTTPS/TLS handshakes; extract visible elements such as the SNI without exposing payloads.
* Export a filtered, sanitized PCAP and create Sigma or Sysmon hunting queries based on observed behaviors.
* Practice capturing on both wired and wireless interfaces and learn how promiscuous mode and monitor mode affect visibility and traffic scope.
