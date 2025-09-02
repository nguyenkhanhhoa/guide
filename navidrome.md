### install

sudo useradd -r -M navidrome
sudo install -d -o navidrome -g navidrome /hdd/0/local/navidrome
sudo install -d -o navidrome -g navidrome /hdd/0/local/lib/navidrome

chown navidrome:navidrome navidrome
chmod u+x,g+x navidrome

navidrome --configfile "/hdd/0/local/lib/navidrome/navidrome.toml"

mkdir /hdd/0/music

chown navidrome:navidrome /hdd/0/music

### create config file  navidrome.toml
echo "\
LogLevel = 'error'
ScanSchedule = '@every 24h'
TranscodingCacheSize = '256MiB'
MusicFolder = '/hdd/0/music'
Port = 45333\
" > /hdd/0/local/lib/navidrome/navidrome.toml

### create service file  navidrome.service

echo "\
[Unit]
Description=Navidrome Music Server and Streamer compatible with Subsonic/Airsonic
After=remote-fs.target network.target
AssertPathExists=/hdd/0/local/lib/navidrome

[Install]
WantedBy=multi-user.target

[Service]
User=navidrome
Group=navidrome
Type=simple
ExecStart=/hdd/0/local/navidrome/navidrome --configfile \"/hdd/0/local/lib/navidrome/navidrome.toml\"
WorkingDirectory=/hdd/0/local/lib/navidrome
TimeoutStopSec=20
KillMode=process
Restart=on-failure

# See https://www.freedesktop.org/software/systemd/man/systemd.exec.html
DevicePolicy=closed
NoNewPrivileges=yes
PrivateTmp=yes
PrivateUsers=yes
ProtectControlGroups=yes
ProtectKernelModules=yes
ProtectKernelTunables=yes
RestrictAddressFamilies=AF_UNIX AF_INET
RestrictNamespaces=yes
RestrictRealtime=yes
SystemCallFilter=~@clock @debug @module @mount @obsolete @reboot @setuid @swap
ReadWritePaths=/hdd/0/local/lib/navidrome

# You can uncomment the following line if you're not using the jukebox This
# will prevent navidrome from accessing any real (physical) devices
#PrivateDevices=yes

# You can change the following line to \`strict\` instead of \`full\` if you don't
# want navidrome to be able to write anything on your filesystem outside of
# /var/lib/navidrome.
ProtectSystem=full

# You can uncomment the following line if you don't have any media in /home/*.
# This will prevent navidrome from ever reading/writing anything there.
#ProtectHome=true

# You can customize some Navidrome config options by setting environment variables here. Ex:
#Environment=ND_BASEURL=\"/navidrome\"\
" > /etc/systemd/system/navidrome.service


###

systemctl enable navidrome --now
