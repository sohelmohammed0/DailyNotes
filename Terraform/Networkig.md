## What is an IP adress?
An IP adress s a unique numerical label assigned to a device on a network to identify and locate it for communication
    - IPV 4 (32 bits)
    - IPV6
- Those 32 bits split in to 
    - Network Portion (This tells ion which subnet you are in)
    - Host Portion (which device inside that subnet)
    - The split is defined by a subnet mask, usually written in CIDR form like /24 


## Public Subnets
- (globally routable): meant to reachable over the public Internet

## Private Subnets
- (not Internet-routable): used in LANs/VPCs; Internet routers drop these unless translated (NAT) 
- The RFC1918 private blocks are:
    1. 10.0.0.0/8
    2. 172.16.0.0/12
    3. 192.168.0.0/16

## Other special ranges:
1. Loopback: 127.0.0.0/8 (Ex: 127.0.0.1)
2. Link Local/APIPA: 169.254.0.0/16
3. Multicast: 224.0.0.0/4
4. CGNAT (carrier grade NAT): 10.64.0.0/10
5. Documentation/test: 192.0.2.0/24, 198.51.100.0/24, 203.0.113.0/24

- "Routable" simply means routers will forward it across the routing domain.
- Punlic IPV4 is routable on the internet
- Private IPV4 is not, unless translated (NAT) at the edge

## Subnet mask:
- A subnet mask defines how many bits are for network and how may for host 
    - Example: 192.168.2.10/24
- subnet mask = 24 bits for network and 8 bits for host = 255.255.255.0

## Networm adress:
- The Network adress identifies the whole network
- It is the first adress in the range











































































128 64 32 16 8 4 2 1 