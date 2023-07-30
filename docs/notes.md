# DN42 configuration notes

## first commands to run

### configuring the firewall (ufw)

```bash
sudo apt update
sudo apt install -y ufw
sudo ufw allow ssh
sudo ufw limit ssh
sudo ufw allow 179/tcp
sudo ufw allow 50000:54000/udp
sudo sed -i 's/DEFAULT_FORWARD_POLICY="DROP"/DEFAULT_FORWARD_POLICY="ACCEPT"/g' /etc/default/ufw
sudo ufw enable
```

### configuring network settings:

details in https://dn42.tk/howto/networksettings.md

```bash
sudo tee /etc/sysctl.d/42-dn42.conf > /dev/null << DN42
# allow ipv4/v6 forwarding on all ifaces
net.ipv4.conf.all.forwarding=1
net.ipv6.conf.all.forwarding=1

# disable rp_filter
net.ipv4.conf.default.rp_filter=0
net.ipv4.conf.all.rp_filter=0

DN42
```
