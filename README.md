# Modern Network Security Threats: A Study Walkthrough

> Personal study notes on network security threats, written in my own words while studying Network Security at UniKL MIIT. For learning and authorised testing only.

## 1. Why network security matters

Networks used to be protected by physical security and closed environments. Today they are open, connected and accessible, which increases the risk of attack. Security aims to protect data and resources, keep the business running, maintain trust and meet legal requirements.

The CIA triad names what is being protected:

| Goal | Meaning | Example |
|---|---|---|
| Confidentiality | No unauthorised disclosure | Student records leaked |
| Integrity | No unauthorised changes | Grades altered |
| Availability | Services stay usable | LMS down during exams |

Threats are commonly grouped as unstructured, structured, external and internal. Security models range from open (easy to use, weak) through restrictive to closed (strict, harder to use).

## 2. The four attack categories

| Category | Goal | Attacker's question |
|---|---|---|
| Reconnaissance | Discover the target | What is exposed? |
| Access | Enter or gain privilege | Can I get in? |
| Malware | Run malicious code | Can I infect or control it? |
| DoS / DDoS | Disrupt availability | Can I make it unusable? |

A typical attack progresses: discover, gain access, establish control, then disrupt, steal or alter. Real incidents often mix categories, and symptoms are clues, not proof. Investigate before concluding.

## 3. Malware

| Type | How it behaves | Spreads by |
|---|---|---|
| Virus | Attaches to a file or program | User action (running or sharing the file) |
| Worm | Exploits vulnerabilities and copies itself | Itself, across networks, without user action |
| Trojan horse | Looks useful or legitimate but enables unauthorised access | Tricks the user into running it. Does not self-replicate |

Other malware: spyware, keyloggers, rootkits, backdoors, ransomware and botnet agents.

### Anatomy of a worm
1. **Enabling vulnerability:** the worm gets in through an exploit.
2. **Propagation:** it copies itself and picks new targets.
3. **Payload:** once installed, the attacker has access, often with privilege-escalation potential.

### Five phases of a worm or virus attack
Probe, penetrate, persist, propagate, paralyze.

### Case studies
- **Code Red (2001):** targeted web servers through a buffer overflow. Its behaviour depended on the date: spread, then flood, then sleep. Timely patching would have prevented most of the damage.
- **SQL Slammer (2003):** spread over UDP port 1434 and slowed global traffic within minutes. The patch already existed, but many servers were not updated.

### Mitigation
- **Viruses and Trojans:** up-to-date antivirus plus host-based intrusion prevention. Cleaning infected machines costs far more than keeping defences current.
- **Active worm, four phases:** containment (limit spread, segment, use ACLs), inoculation (patch uninfected systems), quarantine (find and isolate infected hosts), treatment (disinfect, patch, or reinstall).
- **Infected endpoint:** contain, identify, eradicate, recover, learn. Do not just delete the file and reconnect. Preserve evidence.

## 4. Reconnaissance attacks

Information gathering before an attack. Not always illegal, since the same tools are used in authorised testing, so context and permission matter.

- **Information queries:** whois and nslookup reveal domain ownership and addresses.
- **Ping sweep:** finds which IP addresses map to live hosts.
- **Port scan:** probes ports one by one to see which services respond.
- **Packet sniffing:** captures traffic with the network card in promiscuous mode. Plaintext protocols (FTP, Telnet, TFTP, SNMP) are readable.

**Mitigation:** minimise exposed services, patch, encrypt traffic, use switched networks, deploy IPS, enforce a policy against protocols that are easy to eavesdrop on. Scans cannot be fully prevented on an internet-connected system. Turning off ICMP echo on edge routers stops ping sweeps but also removes useful diagnostics.

## 5. Access attacks

Exploiting weak authentication or trust to retrieve data, gain entry or escalate privileges.

- **Password attacks:** brute force, dictionary attacks, stolen credentials, sniffed passwords. Password cracking tools work from hashes.
- **Trust exploitation:** abusing a trust relationship, for example compromising a DMZ host that an inside host trusts.
- **Port redirection:** a compromised host relays traffic that a firewall would otherwise block (Netcat is a known example).
- **Man-in-the-middle:** intercepting or altering communication for theft, session hijacking, traffic analysis or data corruption.
- **Buffer overflow:** a very common flaw that malware uses to gain privileges.

**Mitigation**
- Passwords: unique passwords, lockout after failed attempts, one-time passwords or MFA, strong passwords, no plaintext.
- Trust: restrict trust levels, and never fully trust systems outside the firewall.
- Man-in-the-middle: encryption (for example an IPSec tunnel) so an interceptor sees only ciphertext.
- Identity logging, least privilege and segmentation help detect and limit damage.

## 6. DoS and DDoS attacks

The target is availability. DoS overwhelms or exhausts a service so legitimate users cannot use it. DDoS does the same from hundreds or thousands of sources, often a botnet.

| Attack | Idea |
|---|---|
| Ping of death | Oversized ICMP packet crashes the target (legacy) |
| Smurf | Spoofed ICMP to broadcast addresses so every host replies to the victim |
| SYN flood | Forged TCP SYNs fill the connection table with half-open connections |
| Volumetric flood | Traffic saturates the network link |
| Application flood | Many expensive requests exhaust the web service or API |

Mitigation must match the bottleneck (link capacity, connection state or application resources): anti-DoS limits on half-open connections, anti-spoofing filters (ACLs, unicast RPF, RFC 3704 filtering), rate limiting with the ISP, DDoS protection services, and spare capacity. No single control stops everything.

Risks include downtime, lost revenue and customers, extortion, and DDoS used as a distraction while data is stolen.

## 7. How an attacker works: seven steps

1. Footprint analysis (reconnaissance): public information, whois, nslookup, traceroute, Nmap.
2. Detail the information: find services and versions, match them to known vulnerabilities.
3. Manipulate users: social engineering (phone pretexting, shoulder surfing, dumpster diving) and password cracking.
4. Escalate privileges.
5. Gather more passwords and secrets.
6. Install backdoors and port redirectors.
7. Leverage the compromised system, then repeat from step 1.

Warning signs of social engineering: urgency, claimed authority, refusal to give a call-back number, name dropping, flattery and out-of-ordinary requests. The defence is user education and verifying identities.

**Defeating footprinting:** keep sensitive data offline, minimise public information, audit your own website, and run your own ping sweep.

## 8. Best practices

- Patch promptly and shut down unnecessary services and ports.
- Use strong passwords and control physical access.
- Validate user input, rejecting characters such as `;`, `<` and `>`.
- Back up and test restores.
- Train users against social engineering.
- Encrypt and password-protect sensitive data.
- Use firewalls, IPS, VPNs, antivirus and content filtering.
- Maintain a written security policy.

## 9. Cisco Network Foundation Protection (NFP)

A router or switch is divided into three planes.

| Plane | Role | How to secure it |
|---|---|---|
| Control | Routing and device-generated packets (ARP, OSPF) | AutoSecure, routing protocol authentication, Control Plane Policing |
| Management | Managing the device (SSH, SNMP, AAA, syslog) | Login and password policy, legal banner, strong-authentication protocols, RBAC, access logging |
| Data (forwarding) | User traffic passing through | ACLs, port security, DHCP snooping, Dynamic ARP Inspection, IP Source Guard |

## 10. Key takeaways

- Attacks fall into four categories, and real incidents often combine them.
- Match controls to the attack: prevent, detect, contain, recover.
- Layered defence beats any single product.
- Patching, strong authentication, encryption and user awareness cover most of the common threats.

---

*Sources: course lecture notes (UniKL MIIT, Network Security, Chapter 1) and Cisco CCNA Security material, summarised and rewritten in my own words.*
