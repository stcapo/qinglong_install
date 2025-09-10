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
