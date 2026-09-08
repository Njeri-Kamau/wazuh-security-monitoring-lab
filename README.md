 # Wazuh Security Monitoring & Threat Hunting Lab

## Project Overview

This project documents a hands-on security monitoring laboratory built using Wazuh, Kali Linux, Metasploitable, Docker, and VulnBank in an isolated Oracle VirtualBox environment.

The primary objective was to gain practical experience with security monitoring, endpoint visibility, log collection, alert investigation, Security Configuration Assessment (SCA), and CIS benchmark findings.

The project also included troubleshooting Wazuh agent-manager communication and investigating log sources from an intentionally vulnerable environment.

---

## Lab Environment

| System | Role | IP Address |
|---|---|---|
| Wazuh Server | Wazuh Manager & Dashboard | `192.168.56.103` |
| Kali Linux | Security workstation / Wazuh agent | `192.168.56.102` |
| Metasploitable | Intentionally vulnerable test system | `192.168.56.101` |
| VulnBank | Intentionally vulnerable web application | Docker |

## Network Connectivity

Connectivity between the systems was validated using ICMP.

From the Wazuh server:

ping -c 4 192.168.56.102

From Kali Linux:

ping -c 4 192.168.56.103

Both tests returned:

4 packets transmitted
4 received
0% packet loss

This confirmed network-level connectivity between the Kali endpoint and Wazuh server.

Network: 192.168.56.0/24

## Metasploitable Integration

Metasploitable was used as an intentionally vulnerable system within the isolated laboratory.

Its IP address was:

192.168.56.101

Syslog forwarding was configured to send logs toward the Wazuh manager:

. @192.168.56.103

The logging daemon was restarted successfully.

## Troubleshooting

The initial investigation showed that:

/etc/rsyslog.conf

was not present.

The available configuration was:

/etc/syslog.conf

This was used for the forwarding configuration.

The available evidence demonstrates that syslog forwarding was configured and the logging service was restarted. It does not prove that every security-testing action against Metasploitable was detected by Wazuh.

## Security Testing Observation

A controlled SSH security-testing attempt was performed from Kali against the Metasploitable laboratory system.

The attempt encountered an SSH cryptographic compatibility error:

no match for method mac algo client->server

The connection therefore did not proceed as a successful authentication attempt.

This was documented as a troubleshooting observation rather than a successful compromise.

## Wazuh Threat Hunting

A Wazuh threat-hunting report was generated covering a 24-hour monitoring period:

2026-09-07 00:58:55
to
2026-09-08 00:58:55

The report identified the Wazuh manager as:

wazuh-server

The report included:

Alert-level evolution
Agent activity
MITRE ATT&CK views
Alert summaries
Security Configuration Assessment findings
CIS benchmark findings

## Selected Alert Findings

Rule ID	Event	Level	Count
5502	PAM login session closed	3	11
80730	Auditd SELinux permission check	3	9
5501	PAM login session opened	3	7
5402	Successful sudo to ROOT executed	3	5
533	Listened-port status changed	7	4
502	Wazuh server started	3	3
19004	CIS benchmark score below 50%	7	2
5403	First-time user executed sudo	4	2

These events demonstrate visibility into authentication, privilege-related, system, and security-configuration activity.

## Security Configuration Assessment

The Wazuh report contained Security Configuration Assessment findings based on CIS benchmarks.

One recorded finding was:

CIS Distribution Independent Linux Benchmark v2.0.0.
Score less than 50% (45)

The report also contained configuration checks covering areas such as:

USB storage
/etc/hosts.deny
AIDE
Avahi
DCCP
ICMP redirects
IP forwarding
IPv6 firewall configuration
Reverse path filtering
SCTP
SELinux
SSH configuration

## Additional Findings

The threat-hunting report also recorded:

New wazuh agent connected.

and:

USB device disconnected

An Amazon Linux 2023 CIS benchmark result was also recorded with a score of:

44

## VulnBank & Docker

VulnBank was deployed as an intentionally vulnerable web application using Docker.

The Docker environment included:

VulnBank web container
PostgreSQL database container
Autoheal container

The web application was exposed locally on port:

5000

The web container was observed responding successfully to health checks such as:

GET /healthz

## Docker Log Investigation

An investigation was performed to determine where VulnBank's container logs were stored.

Instead of assuming a traditional Linux log file, the Docker container configuration was inspected.

The container's JSON log file was identified under:

/var/lib/docker/containers/

The log file was verified to exist.

## Current Status

The Docker log source was successfully identified, but Docker log integration with Wazuh was not completed during this phase.

## Troubleshooting

Several practical troubleshooting situations were encountered during the project.

### 1. Incorrect Wazuh Manager Address

The Wazuh agent contained:

MANAGER_IP

instead of the actual manager address.

The configuration was corrected to:

192.168.56.103

### 2. Network Connectivity

Connectivity was tested between the Wazuh server and Kali and returned:

0% packet loss

### 3. Metasploitable Logging

The expected rsyslog.conf file was not available, so the available syslog.conf configuration was investigated and used.

### 4. SSH Compatibility

The SSH testing attempt encountered a MAC-algorithm compatibility issue.

### 5. Docker Logging

The VulnBank container's actual Docker JSON log location was investigated and identified.

## Skills Demonstrated

### Security Operations
Security monitoring
Alert investigation
Threat-hunting report analysis
Security Configuration Assessment
CIS benchmark analysis
Log-source investigation

### Linux
Linux command line
Service management
Configuration files
Network interface inspection
Log investigation
Troubleshooting

### Networking
IPv4 addressing
Private laboratory networking
ICMP connectivity testing
Port and service verification

### Virtualization & Containers
Oracle VirtualBox
Kali Linux
Metasploitable
Docker
Container log investigation

## Key Lessons

This project demonstrated that security monitoring involves more than installing a monitoring platform.

A functional monitoring environment requires:

Configure
↓
Connect
↓
Validate
↓
Collect
↓
Monitor
↓
Investigate
↓
Document

The project also reinforced the importance of distinguishing between:

Confirmed security events
Normal system activity
Configuration findings
Unsuccessful testing attempts
Confirmed detections

## Limitations

The environment was hosted in Oracle VirtualBox.
Testing was performed within an isolated laboratory.
The available evidence does not prove that every security-testing action against Metasploitable was detected.
The SSH testing attempt encountered a compatibility issue.
VulnBank Docker logs were identified but were not fully integrated into Wazuh during this phase.
Wazuh alerts and CIS findings should not automatically be interpreted as confirmed attacks.

## Future Work

The next phase of the project will focus on VulnBank.

Planned activities include:

Mapping the application's attack surface
Controlled vulnerability testing
Burp Suite testing
Documenting confirmed vulnerabilities
Reviewing application behaviour
Investigating relevant activity in Wazuh
Correlating security-testing activity with available logs
Improving Docker log integration

The longer-term objective is to connect offensive security testing with defensive monitoring and investigation.

## Project Outcome

The lab established a functional foundation for security monitoring using Wazuh.

The project demonstrated practical experience with:

Wazuh manager and agent architecture
Linux administration
Network troubleshooting
Security event monitoring
Threat-hunting reports
SCA/CIS findings
Syslog configuration
Docker environments
Security testing in an isolated laboratory

## Ethical Considerations

All security testing in this project was performed against intentionally vulnerable systems within a controlled virtual laboratory environment.

The purpose of the project was educational: to develop practical skills in vulnerability assessment, security monitoring, logging, investigation, and defensive analysis.

## Evidence

Screenshots and the Wazuh threat-hunting report provide supporting evidence for the configuration, connectivity tests, service status, logging configuration, testing observations, and Wazuh findings documented in this project.

## Conclusion

This project established a practical foundation for security monitoring, threat hunting, Linux administration, networking, logging, and security troubleshooting using Wazuh.

The next phase will focus on VulnBank vulnerability testing and correlating controlled security-testing activity with defensive monitoring and available logs., logging configuration, testing observations, and Wazuh findings documented in this project.
