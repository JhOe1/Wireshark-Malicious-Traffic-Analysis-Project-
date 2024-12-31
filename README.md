# Wireshark Malicious Traffic Analysis Project 

# 



First I had to download and install Wireshark
What is Wireshark?
Wireshark is a widely used, open-source network protocol analyzer that enables users to capture and examine the data travelling through a network in real-time. It provides detailed insights into the network traffic, including packet-level data, which can be crucial for troubleshooting network issues, analyzing performance, detecting security vulnerabilities, and diagnosing network-related problems.
<img width="1678" alt="Screenshot 2024-12-26 at 05 02 16" src="https://github.com/user-attachments/assets/46be4ec7-644a-4b76-8304-ac71defa683c" />

<br>
<br>
<br>

# About the pcap
RedLine Stealer is information-stealing malware that harvests login credentials and sensitive data from a victim's Windows host. This Wireshark quiz uses a packet capture (pcap) that “crosses a line” separating normal traffic from malicious activity. The malicious activity in this pcap is a RedLine Stealer infection from July 2023. Reference if you will read more on the exercise (https://unit42.paloaltonetworks.com/wireshark-quiz-redline-stealer/)

## Scenario
Traffic for this quiz occurred in an Active Directory (AD) environment during July 2023. Details of the local area network (LAN) environment for the pcap follow.

Local Area Network (LAN) Details<br>
LAN segment range: 10.7.10[.]0/24 (10.7.10[.]1 through 10.7.10[.]255)<br>
Domain: coolweathercoat[.]com<br>
Domain controller IP address: 10.7.10[.]9<br>
Domain controller hostname: WIN-S3WT6LGQFVX<br>
LAN segment gateway: 10.7.10[.]1<br>
LAN segment broadcast address: 10.7.10[.]255<br>

## Quiz Questions
What is the date and time in UTC the infection started?<br>
What is the IP address of the infected Windows client?<br>
What is the MAC address of the infected Windows client?<br>
What is the hostname of the infected Windows client?<br>
What is the user account name from the infected Windows host?<br>
What type of information did this RedLine Stealer try to steal?<br>

# Configurations 
The analysis was done on my Virtual Kali machine 
After installing Wireshark, I created a new profile "Updated" and customized the interface to optimize it for efficient analysis and maximize its functionality.
<img width="1320" alt="Screenshot 2024-12-27 at 09 23 37" src="https://github.com/user-attachments/assets/abf33f7f-efba-4824-a508-358cc74242a2" />


I  created three filter expressions to help me investigate the pcaps effectively 

1. Basic "(http.request or tls.handshake.type eq 1) and !(ssdp)"
 This filter allowed me to:
* Capture HTTP requests and TLS "Client Hello" messages, which are crucial for analyzing web communication and secure connections.<br>
* Exclude SSDP traffic, which is often irrelevant in web traffic analysis, ensuring a cleaner and more focused dataset.<br>
* By using this filter, I was able to streamline my analysis and concentrate on packets directly related to web interactions.

<img width="1317" alt="Screenshot 2024-12-28 at 10 45 19" src="https://github.com/user-attachments/assets/e3df012c-739f-413d-ac1f-3eea2c7baf7c" />


<br>
<br>
<br>

2. Basic+ "(http.request or tls.handshake.type eq 1 or tcp.flags eq 0x0002) and !(ssdp)"
 This filter allowed me to:
* Captures TLS "Client Hello" packets, which are the initial messages in establishing secure TLS connections.<br>
* Captures TCP packets with the SYN flag set. The SYN flag is used in the initial stage of the TCP three-way handshake, indicating the start of a new connection ( With this I can detect all connections made ).<br>
* Captures HTTP request packets, representing client requests to a server using the HTTP protocol.<br>

<img width="1316" alt="Screenshot 2024-12-28 at 11 42 44" src="https://github.com/user-attachments/assets/7460d6c5-ab2d-4b92-b0ed-c3a11852d91b" />

<br>
<br>
<br>
 
3. Basic+DNS "(http.request or tls.handshake.type eq 1 or tcp.flags eq 0x0002 or dns) and !(ssdp)"
  This filter allowed me to:
* http.request: Captures HTTP request packets, useful for analyzing web traffic and client-server interactions.
* tls.handshake.type eq 1: Captures TLS "Client Hello" messages, which are part of the secure handshake process for establishing HTTPS connections.

* tcp.flags eq 0x0002: Captures TCP packets with the SYN flag set, indicating the initiation of a TCP handshake.

* dns: Captures all DNS packets, including DNS queries and responses, which are vital for understanding name resolution in network activity (DNS traffic provides insights into which domains devices on the network are attempting to connect to. This information can help identify normal behaviour and detect anomalies).
<img width="1313" alt="Screenshot 2024-12-31 at 04 09 07" src="https://github.com/user-attachments/assets/b8997091-55aa-4f60-aa4c-2479dddb2d73" />



Finally, I exported the configuration profile I created in Wireshark. This step ensures that when setting up Wireshark on another system, I can simply import the saved profile instead of configuring a new one from scratch, saving time and maintaining consistency in my analysis setup.
<img width="1316" alt="Screenshot 2024-12-31 at 04 26 44" src="https://github.com/user-attachments/assets/fb03450f-dc1b-46db-97c8-707366048427" />

# Analysis 
Using the Basic filter I was able to get the IP address of the infected Windows client: 10.7.10.47, The MAC address:80:86:5b:ab:1e:c4 and the date and time in UTC the infection started:2023-07-10 22:39.
<img width="1313" alt="Screenshot 2024-12-31 at 04 53 53" src="https://github.com/user-attachments/assets/6903ae33-d5e6-4a6e-9e43-f1d58de7d490" />



