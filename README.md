Network Reconnaissance Methodology and Analysis

This document outlines the step-by-step process executed for conducting a network reconnaissance scan, mapping services, and performing an initial risk assessment on a local network segment.

The 8-Step Reconnaissance Process

1. Install Nmap from official website.

Action Taken: Downloaded and installed the Nmap (Network Mapper) utility.
Purpose: Nmap is the foundational tool required to send crafted network packets, enabling fast and comprehensive host and port scanning.

2. Find your local IP range (e.g., 192.168.1.0/24).

Action Taken: Used local operating system commands (ipconfig or ip addr) to determine the network interface's IP address and inferred the network's CIDR range as 192.168.29.0/24.
Purpose: To define the scope of the scan and ensure that the Nmap command targets the entire relevant local network segment.

3. Run: nmap -sS 192.168.29.0/24 to perform TCP SYN scan.

Action Taken: Executed the Nmap command using the -sS flag (TCP SYN Scan or "Stealth Scan") across the identified network range.
Purpose: The SYN scan is the most common and efficient scan type. It allows for quick differentiation between open, closed, and filtered ports by initiating the TCP three-way handshake but immediately resetting the connection before completion, making it less intrusive than a full connection scan.

4. Note down IP addresses and open ports found.

Action Taken: Captured the resulting output from Nmap, which identified six active hosts and listed all ports determined to be in the open state, along with Nmap's initial service guess (e.g., http, msrpc).
Purpose: To create the primary data set (raw output) required for subsequent analysis and vulnerability research.

5. Optionally analyze packet capture with Wireshark.

Action Taken: Ran Wireshark simultaneously with the Nmap scan, applying a filter to observe the traffic between the scanning host and the targets. Screenshots were taken of the Wireshark display to visually document the SYN/ACK packet exchanges.
Purpose: To confirm the functionality of the Stealth Scan at the packet level.

For Open Ports, traffic was observed as SYN (Scanner) $\rightarrow$ SYN/ACK (Target) $\rightarrow$ RST (Scanner), confirming the half-open nature.

For Closed Ports, traffic was observed as SYN (Scanner) $\rightarrow$ RST/ACK (Target), confirming the port is actively denying the connection.

6. Research common services running on those ports.

Action Taken: Investigated the ports identified in Step 4, mapping specific port numbers (e.g., 445, 902, 1900) to their corresponding known services and protocols (e.g., SMB, VMware vmauthd, UPnP).
Purpose: To understand the functional role of each listening service, which is essential for determining configuration risk.

7. Identify potential security risks from open ports.

Action Taken: Analyzed the identified services to assess their risk profile, highlighting critical exposures.
Key Findings: The primary risks identified relate to:

Windows File Sharing (Ports 135, 139, 445): High risk due to potential for lateral movement and exploitation (e.g., EternalBlue).

VMware Console Access (Port 902): Risk of hypervisor control if authentication is weak.

UPnP/Device Discovery (Ports 1900, 2869): Risk of firewall bypass via malicious automatic port forwarding.
Purpose: To translate technical findings into actionable security recommendations.

Documents Attached in the GitHub Repo

The following files provide the raw data, evidence, and analysis supporting this reconnaissance project:

Screenshots (Step 5 Evidence)

wireshark_syn_packet.png: Screenshot showing the initial SYN packet sent from the scanning host.

wireshark_syn_ack_packet.png: Screenshot showing the SYN/ACK response from the target host for an open port.

Text Files (Raw Data & Analysis)

nmap_raw_results.txt: The complete, unfiltered output from the nmap -sS scan, including all open and closed port data.

service_mapping_analysis.txt: Documentation of common services mapped to the open ports found (Step 6 data).

potential_risks_summary.txt: Summary of the key security risks and associated reasoning for the open ports (Step 7 data).

