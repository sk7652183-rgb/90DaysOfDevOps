Day 15 – Networking Concepts: DNS, IP, Subnets & Ports

*******Task 1: DNS – How Names Become IPs*************

Explain in 3–4 lines: what happens when you type google.com in a browser?

Answer - When we type google.com in the browser, it first checks the cache for the IP address.
If it is not found, it asks DNS servers to get the IP address.
Then the browser sends a request to Google's server.
The server sends back the webpage, and the browser displays it.

What are these record types? Write one line each


Answer- A Record – Maps a domain name to an IPv4 address.
AAAA Record – Maps a domain name to an IPv6 address.
CNAME Record – Creates an alias, pointing one domain name to another domain name.
MX Record – Specifies the mail server responsible for receiving emails for a domain.
NS Record – Specifies the DNS name servers responsible for a domain.


Ran: dig google.com — identified the A record and TTL from the output

Answer - A Record: 216.58.203.14
TTL (Time To Live): 273 seconds
273 is the TTL value — how long the DNS result can be cached before asking DNS again.

<img width="1854" height="1078" alt="image" src="https://github.com/user-attachments/assets/0b15deaa-0fd9-4936-8a5e-a469f0612186" />


****Task 2: IP Addressing**************

What is an IPv4 address? How is it structured?

Answer - IPv4 is a unique address used to identify a device on a network.
It is structured as four numbers separated by dots (32 bits), for example 192.168.1.1.
Each section (octet) ranges from 0 to 255.

Difference between public and private IPs — give one example of each

Answer- Public IP: An IP address that is accessible over the internet and can be reached from anywhere in the world.
Example: 2.56.22.5

Private IP: An IP address used inside a local/private network and is not directly accessible from the internet.
Example: 192.168.1.1

What are the private IP ranges?

Answer - Private IP ranges are:

10.0.0.0 – 10.255.255.255 (10.x.x.x)
172.16.0.0 – 172.31.255.255 (172.16.x.x – 172.31.x.x)
192.168.0.0 – 192.168.255.255 (192.168.x.x)

Ran: ip addr show — identify which of your IPs are private

Answer - wlo1 → your Wi-Fi network interface
10.65.155.219 → your private IPv4 address
/24 → subnet mask (255.255.255.0)

other private IPs created by Docker:

172.17.0.1 → docker0
172.18.0.1 → Docker bridge network
192.168.49.1 → another bridge/container network


****Task 3: CIDR & Subnetting***********

What does /24 mean in 192.168.1.0/24?

Answer - In 192.168.1.0/24, /24 is the CIDR notation (subnet mask).
It means the first 24 bits are the network part and the remaining 8 bits are for host addresses.
Since 32 − 24 = 8, we get 2⁸ = 256 total IP addresses (192.168.1.0 to 192.168.1.255).

How many usable hosts in a /24? A /16? A /28?

Answer - /24 → 254 usable hosts
/16 → 65534 usable hosts
/28 → 14 usable hosts

Explain in your own words: why do we subnet?

Answer - We use subnetting to divide a large network into smaller networks for better management, improved performance, and reduced network traffic. It also helps with network organization and can improve security by separating different parts of the network.

Quick exercise — fill in:

Answer - | CIDR | Subnet Mask | Total IPs | Usable Hosts |
|------|--------------|------------|---------------|
| /24  | 255.255.255.0 | 256 | 254 |
| /16  | 255.255.0.0 | 65536 | 65534 |
| /28  | 255.255.255.240 | 16 | 14 |

*******Task 4: Ports – The Doors to Services*************

What is a port? Why do we need them?

Answer - A port is a logical number used by a system to identify a specific application or service. We need ports so multiple applications can use the same IP address and network connection.

Document these common ports:

| Port  | Service    |
|--------|------------|
| 22     | SSH        |
| 80     | HTTP       |
| 443    | HTTPS      |
| 53     | DNS        |
| 3306   | MySQL      |
| 5432   | PostgreSQL |
| 6379   | Redis      |
| 27017  | MongoDB    |

Ran ss -tulpn

ss -tulpn is used to check active listening ports and network connections on a Linux system. It shows TCP/UDP ports, services, and the processes using them.

<img width="1854" height="1078" alt="image" src="https://github.com/user-attachments/assets/df7bfe9e-cf2c-4161-85e0-93bb240b9da9" />


***Task 5: Putting It Together************

ran curl http://google.com 

curl http://google.com sends an HTTP request and shows the server’s response. In this output, 301 Moved means the page has been permanently redirected to another URL (http://www.google.com/).

<img width="1854" height="1078" alt="image" src="https://github.com/user-attachments/assets/f3b34a5c-e659-45d7-ba02-a41178b5dae8" />

Your app can't reach a database at 10.0.1.50:3306 — what would you check first?

Answer - First, I would check network connectivity and whether port 3306 is reachable. Then I would verify that the database service is running and listening on port 3306, and check firewall/security rules.



