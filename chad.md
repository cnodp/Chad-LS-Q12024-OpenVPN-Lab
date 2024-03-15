# CHAD Networking: OpenVPN Configs:

Consider the openvpn configs below and answer questions about them. The last question will ask you to write one yourself given a list of requirements. Good luck chads!


# Section 1: Basics

```
proto udp
port 1194

mode server

server 10.8.0.0 255.255.255.0
ifconfig-pool 10.8.0.2 10.8.0.254 255.255.255.0

ca /path/to/your/ca.crt
cert /path/to/your/server.crt
key /path/to/your/server.key
dh /path/to/your/dh2048.pem

cipher AES-256-CBC
auth SHA256

push "dhcp-option DNS 3.3.3.3"
push "dhcp-option DNS 8.8.8.8"
push "dhcp-option DNS 8.8.4.4"


comp-lzo

persist-tun
persist-key

status openvpn-status.log
log-append openvpn.log
verb 3

tls-server
tls-auth ta.key 0
key-direction 0

client-to-client

max-clients 50

keepalive 10 120

user nobody
group nogroup

push "dhcp-option DOMAIN mycompany.local"
push "dhcp-option DOMAIN-SEARCH mycompany.local, yourcompany.local"
push "dhcp-option LOG-SERVER 2.2.2.2"
```



1.1) What is the VPN's subnet?

1.2) What compression algorithm is specified in the config?

1.3) What is the ip range for clients when using this vpn?

1.4) How long will the server wait for a keepalive packet before timing out?

1.5) If a client using this vpn attempts to access a resource with the hostname "research_data" what fully qualified domain name will the client first attempt to resolve?

1.6) If a client were to browse to internal_server, which DNS server would most likely resolve the request?

1.7) If a client attempts to resolve incorrect_data and it is not resolved by the first DNS server, which DNS server will attempt to resolve it? Who owns said DNS server?

	

# Section 2: Routing

```
dev tun
port 1194
proto udp

mode server
topology subnet

server 10.8.0.0 255.255.255.0

ca /path/to/your/ca.crt
cert /path/to/your/server.crt
key /path/to/your/server.key
dh /path/to/your/dh2048.pem

cipher AES-256-CBC
auth SHA256

verb 3

log-append openvpn.log

client-to-client

route 1.1.1.0 255.255.255.0 vpn_gateway
route 2.2.2.0 255.255.255.0 net_gateway
route 3.3.3.0 255.255.255.0 4.4.4.4
```


INFO) This config is a simple example of policy based routing, answer the following routing questions below:

2.1) If the client is attempting to access the IP 2.2.2.2, which gateway will the client be routed to?

2.2) If the client is attempting to access the ip 1.1.1.1, which gateway will the client be routed to?

2.3) If the client is attempting to access the IP 3.3.3.12, which gateway will the client be routed to?



# Section 3: Do-It-Yourself! (CHAD)

Please write an openvpn config with the following specifications:
- add a DNS server at 4.4.4.4 and 3.3.3.3
- your network has more resources relying on 3.3.3.3 so your config must prioitize it for name resolution
- ensure that users of the vpn can still access google services
- add a route to a remote office network (10.9.0.0/24)
- your office which is hosting the VPN doesn't want to see any traffic to 12.43.0.0/16 get routed through the VPN gateway, make sure this traffic goes to the client's gateway rather than through the VPN tunnel

There are different ways to do this, some more straightforward than others, but for the sake of time please use the template below:


```
BOILER PLATE:

server 10.8.0.0 255.255.255.0

# your code goes here:

# Additional OpenVPN Configurations
dev tun
proto udp
port 1194

# Certificates and Keys
ca /path/to/your/ca.crt
cert /path/to/your/server.crt
key /path/to/your/server.key
dh /path/to/your/dh2048.pem

# Additional Security Configurations
cipher AES-256-CBC
auth SHA256

# Configure Logging
log-append openvpn.log
verb 3

# Enable Client-to-Client communication
client-to-client

# Keep-alive interval and timeout
keepalive 10 120

# Compression
comp-lzo

# Set the user and group to run as
user nobody
group nogroup
```
