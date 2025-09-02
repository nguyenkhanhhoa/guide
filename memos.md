#Build   
cd web
pnpm install   
pnpm release   


cd ..
# Clean Go module cache
go clean -modcache
# Tidy modules
go mod tidy
# Update dependencies
go get -u ./...
go build -ldflags "-s -w" -o "build/memos" ./bin/memos/main.go


>
#Tranfer    
scp memos hoanguyen@192.168.1.2:/home/hoanguyen/downloads

#Extract the archive
tar -xzf memos_*.tar.gz


#Move to system location (Linux/macOS)  
sudo mv memos /usr/local/bin/  
#Make executable  
sudo chmod +x /usr/local/bin/memos  
>

#Create config directory   
sudo mkdir -p /etc/memos   

#Create environment file
sudo tee /etc/memos/memos.env << EOF
MEMOS_MODE=prod
#MEMOS_UNIX_SOCK=/var/lib/memos/memos.sock
MEMOS_ADDR=127.0.0.1
MEMOS_PORT=5230
#MEMOS_INSTANCE_URL=
MEMOS_DATA=/var/lib/memos
MEMOS_DRIVER=postgres
MEMOS_DSN=postgresql://user:pass@localhost/memos
EOF

#Create data directory    
sudo mkdir -p /var/lib/memos   

#Create memos user (recommended)    
sudo useradd --system --home /var/lib/memos --shell /bin/false memos

#Set ownership    
sudo chown -R memos:memos /var/lib/memos

#Create service

sudo tee /etc/systemd/system/memos.service << 'EOF'
[Unit]
Description=Memos
After=network.target
[Service]
Type=simple
User=memos
Group=memos
WorkingDirectory=/var/lib/memos
EnvironmentFile=/etc/memos/memos.env
ExecStart=/usr/local/bin/memos
Restart=always
RestartSec=3
StandardOutput=journal
StandardError=journal
#Security settings
NoNewPrivileges=true
PrivateTmp=true
ProtectHome=true
ProtectSystem=strict
ReadWritePaths=/var/lib/memos
[Install]
WantedBy=multi-user.target
EOF


#Reload systemd
sudo systemctl daemon-reload

#Enable auto-start
sudo systemctl enable memos

#Start service
sudo systemctl start memos

#Check status
sudo systemctl status memos

CREATE USER memos;

CREATE DATABASE "memos"
WITH
  OWNER = "memos"
  ENCODING = 'UTF8'
;


# Nginx

server {
    listen 80;
    server_name memos.example.com;
    
    location / {
        proxy_pass http://localhost:5230;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        
        # WebSocket support
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}

