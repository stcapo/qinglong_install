# qinglong_install
for both of domestic and aboard vps

# for oversea vps
docker run -dit \
  -v $PWD/ql/data:/ql/data \
  -p 5700:5700 \
  -e QlBaseUrl="/" \
  -e QlPort="5700" \
  --name qinglong \
  --hostname qinglong \
  --restart unless-stopped \
  whyour/qinglong:latest
### 6dy script
ql repo https://github.com/6dylan6/jdpro.git "jd_|jx_|jddj_" "backUp" "^jd[^_]|USER|JD|function|sendNotify|utils"
right config:
https://github.com/6dylan6/jdpro/issues/22

# for domestic vps

1. set proxy
   xray :
sudo apt update
# put zip into /root
sudo mkdir -p /usr/local/bin
sudo unzip /root/Xray-linux-64.zip -d /usr/local/bin
sudo chmod +x /usr/local/bin/xray
# get your config.json
sudo mkdir -p /usr/local/etc/xray
sudo cp /root/a.json /usr/local/etc/xray/config.json
sudo chown -R root:root /usr/local/etc/xray
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

sudo systemctl daemon-reload
sudo systemctl enable --now xray
sudo systemctl status xray --no-pager

# docker daemon proxy
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

docker run -dit \
  -v $PWD/ql/data:/ql/data \
  -p 5700:5700 \
  -e QlBaseUrl="/" \
  -e QlPort="5700" \
  --name qinglong \
  --hostname qinglong \
  --restart unless-stopped \
  whyour/qinglong:latest
### 6dy script
ql repo https://github.com/6dylan6/jdpro.git "jd_|jx_|jddj_" "backUp" "^jd[^_]|USER|JD|function|sendNotify|utils"
right config:
https://github.com/6dylan6/jdpro/issues/22
