DVWA Security Assessment 

This project demonstrates a comprehensive offensive-to-defensive lifecycle executed against the Damn Vulnerable Web Application (DVWA) in a secure virtualized network. It spans mapping, exploiting, monitoring, and systematically hardening the target application.


Virtual Environment Architecture
Hypervisor: VirtualBox (isolated Host-Only Network adapter scheme)
Attacker Node: Kali Linux (VM)
Target Server: Apache Web Server running DVWA 
Security Controls Analyzed: OpenSSL Local Certificates, Metasploit.


Execution and Hardening Steps

Phase 1: Reconnaissance and Mapping
Conducted deep scanning to discover running services, operating system versions, and SSL deployment states.
Command: `nmap -sV -sC -p- 192.168.xx.xxx`
Result: Detected port `80` (HTTP) and port `22` (SSH) open. Noted missing HTTPS configuration, exposing session cookies to local sniffing attacks.

Phase 2: Vulnerability Exploitation (XSS and SQLi)
Vulnerability Exploitation: Validated vulnerable endpoints using SQL injection techniques to bypass login controls.
Session Sniffing: Captured HTTP packets on the local interface during a simulated login using Wireshark. Since SSL was absent, authentication cookies and user passwords were captured clearly in plain text within HTTP POST requests.

Phase 3: HTTPS Implementation and Certificate Generation
To secure traffic from sniffing vectors, I configured Apache to run exclusively over SSL/TLS.
1.  Generated Self-Signed SSL Certificates:
2.  Configured Apache SSL Module: Enabled `ssl` and `headers` modules in Apache, and created a secure virtual host file targeting port `443` with active certificate directories.
3.  Result Verification: Successfully ran DVWA securely over `https://192.168.xx.xxx/dvwa/`. Packet capture verification confirmed that credential payloads and session cookies are completely encrypted over transit.


Hardening Summary Table

Identified Risk | Exposure Level | Mitigation Action Applied |

Plaintext HTTP Traffic | Critical | Implemented SSL/TLS Certificates; Enforced HTTP to HTTPS redirection. |
Directory Indexing Enabled | Low | Disabled directory indexing within `/etc/apache2/apache2.conf`. |
Insecure PHP Error Reporting | Medium | Modified `php.ini` (`display_errors = Off`) to prevent system path leaks. |
