Day 14 – Networking Fundamentals & Hands-on Checks


***Quick Concepts (write 1–2 bullets each)***

OSI layers (L1–L7) vs TCP/IP stack (Link, Internet, Transport, Application)


"The OSI model consists of 7 layers and is a conceptual/reference model. The TCP/IP stack consists of 4 layers and is a practical model used in real-world networking."

Fact check:
✔️ OSI model → 7 layers (L1–L7)
✔️ TCP/IP stack → 4 layers (Link, Internet, Transport, Application)
✔️ OSI = theoretical/conceptual model
✔️ TCP/IP = practical implementation used on the Internet


**Where IP, TCP/UDP, HTTP/HTTPS, DNS sit in the stack****

IP → Network Layer (OSI L3) / Internet Layer (TCP/IP)
TCP / UDP → Transport Layer (OSI L4 / TCP/IP Transport Layer)
HTTP / HTTPS → Application Layer (OSI L7 / TCP/IP Application Layer)
DNS → Application Layer (OSI L7 / TCP/IP Application Layer)


****One real example: “curl https://example.com = App layer over TCP over IP”***

curl https://google.com is an example of the Application layer (HTTP/HTTPS) running over TCP, which runs over IP.


****Hands-on Checklist (run these; add 1–2 line observations)****

Ran the hostname -I command to check the local IP address and used ip addr to view detailed network interface information

127.0.*.* / 127.0.*.* → Loopback / localhost (not your network IP)
172.17.*.* → Docker bridge IP
172.18.*.* → Docker network IP
192.168.*.* → Another bridge/network interface
192.168.*.*** (on interface wlo1) 

<img width="1920" height="1078" alt="image" src="https://github.com/user-attachments/assets/eb83f286-e2ff-4823-af46-c7d7e31c810e" />

Reachability: ping <target>

Executed the ping google.com command and observed the following results: 25 packets transmitted, 25 received, 0% packet loss, total time 24027 ms, with RTT min/avg/max/mdev values of 32.027/50.073/195.518/37.483 ms.

<img width="1920" height="719" alt="image" src="https://github.com/user-attachments/assets/d372ee15-8c02-48a7-8afc-47c413dbe8dd" />

Path: traceroute <target> (or tracepath) 

Executed the traceroute google.com command to trace the network path to Google. The output showed that the packets traveled through multiple network hops, starting from the local router (192.168.29.1), passing through several private and public IP addresses, and finally reaching google.com (142.250.182.206) in 18 hops. Some hops displayed * * *, indicating no response from intermediate routers. The average response time ranged from approximately 7 ms to 38 ms.


<img width="1856" height="478" alt="image" src="https://github.com/user-attachments/assets/3ec09e3d-3197-41ee-84cc-56e505d703df" />


Ports :

Ran the ss -tulpn command to display active TCP/UDP listening ports and associated processes. The output showed services listening on different ports, including DNS (53), SSH (22), HTTP (80), printing service (631), and Chrome UDP connections (5353). Both IPv4 (0.0.0.0) and IPv6 ([::]) addresses were present.


<img width="1861" height="581" alt="image" src="https://github.com/user-attachments/assets/81fdd94e-f508-47c7-a12f-ad6aba017b92" />


Name resolution:

Executed the dig google.com command to query DNS records for google.com. The output returned an A record mapping google.com to the IP address 142.250.182.110. The DNS query completed successfully with status NOERROR, using the local DNS resolver 127.0.0.53, and the response time was 13 ms.

Executed the nslookup scheltagroup.com command to resolve the domain name. The lookup failed with the result NXDOMAIN, indicating that the domain could not be found in DNS records.


<img width="1913" height="594" alt="image" src="https://github.com/user-attachments/assets/b449614f-0d63-4769-874b-ec81921e6de7" />

HTTP check

The curl -i command displayed both HTTP headers and page content. Google returned a 301 redirect response, informing the client that the requested page has permanently moved to www.google.com.

Connections snapshot: netstat -an | head

Ran netstat -an | head -10 to view active TCP connections and listening ports. Observed local services listening on ports 53 (DNS), 22 (SSH), 80 (HTTP), and 631 (printing service). Also observed an established HTTPS connection from 192.168.29.144 to 140.82.114.22:443

<img width="1862" height="397" alt="image" src="https://github.com/user-attachments/assets/c3cf6695-8999-45f8-b28b-6be7b13c8d22" />

Write one line: is it reachable? If not, what’s the next check? (e.g., service status, firewall).

Port 22 (SSH) is reachable if the service is running and accessible; if not, check the service status (systemctl status ssh) and firewall rules (ufw status or iptables -L).

<img width="1857" height="1073" alt="image" src="https://github.com/user-attachments/assets/8b6cd1fb-aa47-4c76-ba7a-09efc7512a6c" />

Which command gives you the fastest signal when something is broken?

ping gives the fastest signal when something is broken, as it quickly checks basic network connectivity and host reachability.

What layer (OSI/TCP-IP) would you inspect next if DNS fails? If HTTP 500 shows up?

If DNS fails: Inspect the Application Layer (DNS operates at the Application layer in OSI/TCP-IP).
If HTTP 500 shows up: Inspect the Application Layer, since an HTTP 500 error indicates a server-side application/web server issue.












