# tested: Works!
guide: https://wiki.postmarketos.org/wiki/USB_Internet
device: Amazon Fire HD 8 (2018) karnak
guest os: postmarketOS
host os: Ubuntu 23.04 (x86_64)

guest:
```
ip a
ip route add default via 172.16.42.2 dev rndis0
echo nameserver 1.1.1.1 > /etc/resolv.conf
```

host:
```
sysctl net.ipv4.ip_forward=1
iptables -A FORWARD -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
iptables -A FORWARD -s 172.16.42.0/24 -j ACCEPT
iptables -A POSTROUTING -t nat -j MASQUERADE -s 172.16.42.0/24
iptables-save #Save changes
ufw

****# Use ansible Lineinfile & Replace:
## begin ansible
nano /etc/default/ufw
nano /etc/ufw/sysctl.conf
nano /etc/ufw/before.rules
## end ansible

ufw disable && ufw enable
```
