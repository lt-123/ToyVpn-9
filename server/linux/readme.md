# ToyVPN Server

There are several ways to play with this program. Here we just give an
example for the simplest scenario. Let us say that a Linux box has a
public IPv4 address on eth0. Please try the following steps and adjust
the parameters when necessary.

```bash
# Enable IP forwarding
echo 1 > /proc/sys/net/ipv4/ip_forward

# Pick a range of private addresses and perform NAT over eth0.
iptables -t nat -A POSTROUTING -s 172.16.0.0/8 -o eth0 -j MASQUERADE

# Create a TUN interface.
ip tuntap add dev tun1 mode tun

# Set the addresses and bring up the interface.
ifconfig tun1 172.16.0.1 dstaddr 172.16.0.2 up

# Create a server on port 8000 with shared secret "test".
./ToyVpnServer tun1 8000 test -m 1400 -a 172.16.0.2 32 -d 114.114.114.114 -r 0.0.0.0 0
```

This program only handles a session at a time. To allow multiple sessions,
multiple servers can be created on the same port, but each of them requires
its own TUN interface. A short shell script will be sufficient. Since this
program is designed for demonstration purpose, it performs neither strong
authentication nor encryption. DO NOT USE IT IN PRODUCTION!

