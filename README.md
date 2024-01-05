# AS4242421014

This repository holds my configuration files for AS4242421014 in the dn42 registry

Looking glass: <https://lg.dn42.astutus.org/>

## IPv6 range

```text
Network: fd2d:5027:1014:0000:0000:0000:0000:0000
Shorthand: fd2d:5027:1014::
CIDR: 48
Mask: ffff:ffff:ffff:0000:0000:0000:0000:0000
Range: fd2d:5027:1014:0000:0000:0000:0000:0000 - fd2d:5027:1014:ffff:ffff:ffff:ffff:ffff
Total addresses in range: 1.2089258196146292e+24
```

## IPv4 range

```text
Network: 172.23.114.128
CIDR: 27
Mask: 255.255.255.224
Range: 172.23.114.128 - 172.23.114.159
Total addresses in range: 32
```

## Deploying a new node

With valid IPv4/v6 internet connectivity, do the following steps:

```bash

sudo tee /etc/sysctl.d/42-dn42.conf > /dev/null << END
# allow ipv4/v6 forwarding on all ifaces
net.ipv4.conf.all.forwarding=1
net.ipv6.conf.all.forwarding=1

# disable rp_filter
net.ipv4.conf.default.rp_filter=0
net.ipv4.conf.all.rp_filter=0
END

sudo chmod a+x /home/ubuntu # needed otherwise config symlinks fail

sudo ln -s /home/${USER}/dn42-config/$(hostname)/wireguard/ /etc/
sudo ln -s /home/${USER}/dn42-config/$(hostname)/bird/ /etc/
sudo ln -s /home/${USER}/dn42-config/$(hostname)/interfaces.d/ /etc/network/
sudo cp /home/${USER}/dn42-config/$(hostname)/systemd-network/* /etc/systemd/network/

sudo systemctl reload systemd-networkd

sudo apt update && sudo apt upgrade -yy && reboot
sudo apt install -y inetutils-ping traceroute git vim
curl -Lo go_installer https://get.golang.org/linux && chmod +x go_installer && ./go_installer && rm go_installer
source ~/.bash_profile

sudo apt install -y wireguard bird2

go install github.com/xddxdd/bird-lg-go/proxy@latest
sudo systemctl link dn42-config/bird-lg-go/bird-lg-proxy.service
sudo systemctl enable --now bird-lg-proxy.service

```
