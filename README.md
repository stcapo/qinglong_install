# qinglong_install

For both domestic and overseas VPS

## For Overseas VPS

```bash
docker run -dit \
  -v $PWD/ql/data:/ql/data \
  -p 5700:5700 \
  -e QlBaseUrl="/" \
  -e QlPort="5700" \
  --name qinglong \
  --hostname qinglong \
  --restart unless-stopped \
  whyour/qinglong:latest
```

### 6dy Script

```bash
ql repo https://github.com/6dylan6/jdpro.git "jd_|jx_|jddj_" "backUp" "^jd[^_]|USER|JD|function|sendNotify|utils"
```

**Configuration Reference:**  
https://github.com/6dylan6/jdpro/issues/22

## For Domestic VPS

### 1. Set Proxy (Xray)

```bash
sudo apt update
# Put zip file into /root directory
sudo mkdir -p /usr/local/bin
sudo unzip /root/Xray-linux-64.zip -d /usr/local/bin
sudo chmod +x /usr/local/bin/xray

# Setup config file
sudo mkdir -p /usr/local/etc/xray
sudo cp /root/a.json /usr/local/etc/xray/config.json
sudo chown -R root:root /usr/local/etc/xray

# Create systemd service
sudo tee /etc/systemd/system/xray.service > /dev/null <<EOF
[Unit]
Description=Xray Service
After=network.target

[Service]
ExecStart=/usr/local/bin/xray -config /usr/local/etc/xray/config.json
Restart=on-failure
User=root

[Install]
WantedBy=multi-user.target
EOF

# Start and enable service
sudo systemctl daemon-reload
sudo systemctl enable --now xray
sudo systemctl status xray --no-pager
```

### 2. Configure Docker Daemon Proxy

```bash
sudo mkdir -p /etc/systemd/system/docker.service.d
sudo tee /etc/systemd/system/docker.service.d/proxy.conf > /dev/null <<EOF
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:10808"
Environment="HTTPS_PROXY=http://127.0.0.1:10808"
Environment="NO_PROXY=localhost,127.0.0.1"
EOF

sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl restart docker
```

### 3. Run Qinglong Container

```bash
docker run -dit \
  -v $PWD/ql/data:/ql/data \
  -p 5700:5700 \
  -e QlBaseUrl="/" \
  -e QlPort="5700" \
  --name qinglong \
  --hostname qinglong \
  --restart unless-stopped \
  whyour/qinglong:latest
```

### 4. Add 6dy Script

```bash
ql repo https://github.com/6dylan6/jdpro.git "jd_|jx_|jddj_" "backUp" "^jd[^_]|USER|JD|function|sendNotify|utils"
```

**Configuration Reference:**  
https://github.com/6dylan6/jdpro/issues/22
