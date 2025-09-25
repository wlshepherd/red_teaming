# 📘 Network Penetration Testing Notes

## 🔍 1. Discovering Hosts
### ICMP (Internet Control Message Protocol)
The most simplistic method to discover whether a host is up or not. By sending Echo Requests (aka ping), you can easily determine the status of all hosts. If a host returns with an Echo Reply, it's online. This can be both executed within a network and outside, e.g. you can ping Google. However some hosts block ICMP traffic for security reasons, so a lack of a reply does not always mean the host is down.
```bash
ping -c 1 199.66.11.4 # Single echo request to a host, -c flag sets number of requests.
```
```bash
fping -g 199.66.11.0/24  # Pings every IP address in the specified subnet.
```
```bash
nmap -PE -PM -PP -sn -n 199.66.11.0/24 # Sends echo, timestamp requests, and subnet mask requests.
```

### TCP Port Discovery
Each host has 65,535 ports, to be able to test if every port is open or not, this will take long time to complete. The best way to address this issue is to utilise a tool like masscan, and a list of the most common ports.
```bash
masscan -p20,21-23,25,53,80,110,111,135,139,143,443,445,993,995,1723,3306,3389,5900,8080 199.66.11.0/24
```
```bash
masscan -p80,443,8000-8100,8443 199.66.11.0/24 # A scan specific for discovering HTTP services.
```

### UDP Port Discovery
Unlike TCP, UDP mostly don't respond with any data to a regular empty UDP probe packet so it's difficult to say if a port is being filtered or open. Nmap uses several strategies, in order to improve the accuracy. Common services that we look for in a UDP scan are DNS, SNMP and DHCP. If there is no response, the port is assumed to be open and a UDP packet specific to the service on that port is sent to detect the service. However, if an ICMP error packet is returned the port is considered closed. If sucessfully executed, this scan can reveal potential targets.
```bash
nmap -sU -T4 --top-ports 50 --version-intensity 9 -v <target>
```



