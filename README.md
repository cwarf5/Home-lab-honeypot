# Home Lab Honeypot

## Project Summary
Deployed a Teapot/T-Pot honeypot on an isolated Homelab network to gain hands-on experience with threat data collection, firewall segmentation, and log/alert visualization. The deployment captured attacker activity (simulated and real) and was integrated with the ELK/Kibana dashboards provided by the T-Pot stack. This project demonstrates practical skills in virtual machine setup, containerized honeypot deployment, firewall rule creation (pfSense), and basic attacker emulation.

---

## High-Level Architecture
- **Host**: Homelab hypervisor (local) running multiple VMs.
- **Honeypot VM**: Ubuntu Server VM running the T-Pot honeypot (Docker containers).
- **Attack VM (test box)**: Ubuntu VM on same internal network used to run `nmap`, `hydra`, and banner grabs to simulate attacks.
- **Network**: `Internal Network` virtual switch. pfSense provides DHCP and enforces segmentation via firewall rules.
- **Logging / UI**: T-Pot’s ELK stack (Elasticsearch + Logstash + Kibana) accessible via the T-Pot web interface.

---

## What I Did
1. Provisioned **Ubuntu Server** VM (8 GB RAM, 4 CPUs, 128 GB SSD).
2. Installed Ubuntu Server and cloned the T-Pot repository.
3. Ran the T-Pot installer and rebooted the VM.
4. Logged into the T-Pot web UI (example): `https://192.168.1.102:64297`.
5. Configured networking with `Internal Network`, using pfSense for DHCP.
6. Ran simulated attacks (`nmap`, SSH banner grabs) from the attack VM.
7. Observed events populate in Kibana and Cowrie (SSH honeypot).
8. Detected unexpected inbound traffic from external IPs; temporarily shut off honeypot and adjusted pfSense firewall rules.
9. Created firewall rules to block outbound access from honeypot (with temporary exceptions for container initialization).
10. Installed `hydra` on the attack VM to test brute force attempts against the honeypot.

---

## Key Outcomes & Observations
- Kibana dashboards populated successfully with simulated attack events.
- Cowrie logs showed repetitive SSH attempts during banner grabs.
- External IPs probed the honeypot once it was left exposed, confirming visibility to outside traffic.
- pfSense rules successfully blocked outbound traffic when applied, ensuring network segmentation.
- `hydra` brute-force testing revealed that the password string `cat` allowed SSH login to the honeypot, despite the real system password being different — likely a deliberate emulation feature of Cowrie.

---

## Troubleshooting Notes
- **Docker containers not initializing**: Allowed temporary outbound access for initial pulls/startup, then blocked again.
- **Firewall validation**: Used attack VM to test pings and connectivity before applying rules to honeypot.
- **Unexpected SSH access (`cat`)**: Honeypots often simulate login success to capture attacker post-authentication behavior.

---

## Configuration & Security Considerations
- Keep honeypot in a **dedicated network segment** with strict firewall rules.
- Regularly monitor dashboards and consider exporting logs for further analysis.

---

## What I Learned
- Containerized honeypot deployment with T-Pot.
- Real-time attack data visualization with Kibana/ELK.
- Firewall rule creation and validation in pfSense.
- Simulating attacker behavior with scanning and brute-forcing tools.
- Observing how honeypots emulate and log malicious behavior.

---

## Next Steps
- Spin up a Kali attacker VM for broader toolset testing.
- Harden firewall rules (IDS/IPS, rate limiting, geo-blocking).
- Export and parse logs into IOCs.
- Add alerting for specific honeypot events.
- Investigate Cowrie’s credential handling to understand `cat` login behavior.

---

## Tags
`honeypot` `tpot` `cowrie` `kibana` `elk` `pfSense` `homelab` `network-security`
