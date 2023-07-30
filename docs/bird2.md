# Bird2 configuration

## ROA script

```bash
sudo tee /etc/systemd/system/dn42-roa.service > /dev/null <<DN42
[Unit]
Description=Update DN42 ROA

[Service]
Type=oneshot
ExecStart=curl -sfSLR -o /etc/bird/roa_dn42.conf -z /etc/bird/roa_dn42.conf https://dn42.burble.com/roa/dn42_roa_bird2_4.conf
ExecStart=curl -sfSLR -o /etc/bird/roa_dn42_v6.conf -z /etc/bird/roa_dn42_v6.conf https://dn42.burble.com/roa/dn42_roa_bird2_6.conf
ExecStart=birdc configure
DN42

sudo tee /etc/systemd/system/dn42-roa.timer > /dev/null <<DN42
[Unit]
Description=Update DN42 ROA periodically

[Timer]
OnBootSec=2m
OnUnitActiveSec=15m
AccuracySec=1m

[Install]
WantedBy=timers.target
DN42

sudo systemctl daemon-reload
sudo systemctl enable --now dn42-roa.timer
```
