
#
https://github.com/qbittorrent/qBittorrent/wiki/Running-qBittorrent-without-X-server-(WebUI-only,-systemd-service-set-up,-Ubuntu-15.04-or-newer) \



# Ubuntu
adduser  qbt-nox \
passwd -d qbt-nox

### service
sudo nano /etc/systemd/system/qbittorrent-nox.service

[Unit]
Description=qBittorrent Command Line Client
After=network.target

[Service]
Type=forking
User=qbt-nox
Group=qbt-nox
UMask=007
ExecStart=/usr/bin/qbittorrent-nox -d
Restart=on-failure

[Install]
WantedBy=multi-user.target

sudo systemctl daemon-reload

sudo systemctl start qbittorrent-nox

systemctl status qbittorrent-nox


# Nginx SSL
https://github.com/qbittorrent/qBittorrent/wiki/Linux-WebUI-HTTPS-with-Let's-Encrypt-certificates-and-NGINX-SSL-reverse-proxy