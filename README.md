## OpenVPN- Cisco
###### لینک ویدیو :
```
```

###### خرید سرور از دیجیتال اوشن : 
```
https://m.do.co/c/0fb522deafa4
```
###### خرید سرور از سایت ایرانی : 
```
https://dashboard.azaronline.com/order/?aff=790&p=vps
```
###### خرید دامنه از نیم چیپ: 
```
https://namecheap.pxf.io/BX7m6W
```
###### خرید دامنه سایت ایرانی: 
```
https://dashboard.azaronline.com/order/?aff=790&p=domain
```

**If you think this project is helpful to you, you may wish to give a** 🌟

**Feel Free To Donation :** ❤️

>TRC20: ```TGTyqv2MH7dZztMvaP5PKuS9Bma8RY5Pk8```

>ETH: ```0x5b5202a54e5ce4fb25f0d886254eeb07bb088614```
## EU-Server-CentOS
#### Update Server
```
yum update -y
```

#### Install BBR on CentOS
```
yum install wget -y
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
wget http://www.elrepo.org/elrepo-release-7.0-3.el7.elrepo.noarch.rpm
yum -y install ./elrepo-release-7.0-3.el7.elrepo.noarch.rpm
yum list available --disablerepo='*' --enablerepo=elrepo-kernel
yum install -y --enablerepo=elrepo-kernel kernel-ml
awk -F\' /^menuentry/{print\$2} /boot/grub2/grub.cfg
grub2-set-default 0
grub2-mkconfig -o /boot/grub2/grub.cfg
grub2-editenv list
reboot
```

###### Then apply the following Command
```
echo "net.ipv4.ip_forward = 1" | sudo tee /etc/sysctl.d/60-custom.conf
echo "net.core.default_qdisc=fq" | sudo tee -a /etc/sysctl.d/60-custom.conf
echo "net.ipv4.tcp_congestion_control=bbr" | sudo tee -a /etc/sysctl.d/60-custom.conf
sudo sysctl -p /etc/sysctl.d/60-custom.conf
```

#### Install ocserv
```
yum install net-tools epel-release radcli bzip2 wget git nano certbot -y
git clone https://github.com/amirmnoohi/VPN-using-cisco-ocserv.git && cd VPN-using-cisco-ocserv/
chmod +x ocserv.sh
sed -i -e 's/\r$//' ocserv.sh
./ocserv.sh
```

#### Stop & Disable Firewall
```
systemctl stop firewalld
systemctl disable firewalld
```

#### Install certBot
```
yum install certbot -y
certbot certonly --standalone --non-interactive --preferred-challenges http --agree-tos --email YOUR-EMAIL -d YOUR-DOMAIN
```

#### Change Dir of Key & Cert
```
cp /etc/letsencrypt/live/YOUR-DOMAIN/fullchain.pem fullchain.pem
cp /etc/letsencrypt/live/YOUR-DOMAIN/privkey.pem private.key
```

#### OCserv Config
```
nano /etc/ocserv/ocserv.conf
```

#### Change the following date, search by Ctrl+w
```
auth = "plain[passwd=/etc/ocserv/ocpasswd]"
#udp-port = 443
server-cert = /root/fullchain.pem
server-key =  /root/private.key
max-clients = 1024
max-same-clients = 3
keepalive = 30
try-mtu-discovery = true
#idle-timeout=1200
#mobile-idle-timeout=1800
#auth-timeout = 240
default-domain = vpn.example.com
max-ban-score = 0
```

#### Reset OCserv
```
service ocserv restart
```
#### Status OcServ
```
service ocserv status
```
#### MASQUERADE
```
iptables -t nat -A POSTROUTING -j MASQUERADE
```

#### Create new User
```
ocpasswd -c /etc/ocserv/ocpasswd username
```

#### STunnel
```
yum install stunnel openssl -y
```
#### Config STunnel
```
nano /etc/stunnel/stunnel.conf
```
#### Paste the following command
```
cert = /etc/stunnel/stunnel.pem
pid = /etc/stunnel/stunnel.pid
output = /etc/stunnel/stunnel.log

[Cisco]
accept = 58694
connect = 0.0.0.0:443
```
#### Generate Privekey for STunnel
```
openssl genrsa -out privkey.pem 2048
```
#### Cert for STunnel
#### Press Enter for remain question
```
openssl req -new -x509 -key privkey.pem -out cacert.pem -days 1095
```
#### Add to Dir
```
cat privkey.pem cacert.pem >> /etc/stunnel/stunnel.pem
```
#### Change the Permission
```
chmod 0400 /etc/stunnel/stunnel.pem
```
#### Creat Stunnel service
```
nano /usr/lib/systemd/system/stunnel.service
```
#### Paste the following command
```
[Unit]
Description=SSL tunnel for network daemons
After=network.target
After=syslog.target

[Install]
WantedBy=multi-user.target
Alias=stunnel.target

[Service]
Type=forking
ExecStart=/usr/bin/stunnel /etc/stunnel/stunnel.conf
ExecStop=/usr/bin/pkill stunnel

# Give up if ping don't get an answer
TimeoutSec=600

Restart=always
PrivateTmp=false
```
#### Start Stunnel
```
sudo systemctl start stunnel.service
```
#### Enable Stunnel
```
sudo systemctl enable stunnel.service
```

## IRAN-Server-Ubunto
#### Update & Upgrade
```
apt update && apt upgrade -y
```
#### Install BBR
```
echo "net.ipv4.ip_forward = 1" | sudo tee /etc/sysctl.d/60-custom.conf
echo "net.core.default_qdisc=fq" | sudo tee -a /etc/sysctl.d/60-custom.conf
echo "net.ipv4.tcp_congestion_control=bbr" | sudo tee -a /etc/sysctl.d/60-custom.conf
sudo sysctl -p /etc/sysctl.d/60-custom.conf
```

#### Install Stunnel
```
apt install stunnel4 openssl -y
```
#### Stunnel configuration
```
nano /etc/stunnel/stunnel.conf
```
#### Paste the following command
```
pid = /etc/stunnel/stunnel.pid
client = yes
output = /etc/stunnel/stunnel.log

[Cisco]
accept = 443
connect = EU-Server:58694
```
#### Stunnel Service
```
nano /usr/lib/systemd/system/stunnel.service
```
#### Paste the following command
```
[Unit]
Description=SSL tunnel for network daemons
After=network.target
After=syslog.target

[Install]
WantedBy=multi-user.target
Alias=stunnel.target

[Service]
Type=forking
ExecStart=/usr/bin/stunnel /etc/stunnel/stunnel.conf
ExecStop=/usr/bin/pkill stunnel

# Give up if ping don't get an answer
TimeoutSec=600

Restart=always
PrivateTmp=false
```
#### Edit OpenSSl
```
nano /etc/ssl/openssl.cnf
```
#### Add the Following command on the top of the commands
```
openssl_conf = default_conf
```
#### Add the following commands on the end of commands, use Ctrl+V to navigate between pages
```
[default_conf]
ssl_conf = ssl_sect

[ssl_sect]
system_default = system_default_sect

[system_default_sect]
MinProtocol = TLSv1.2
CipherString = DEFAULT@SECLEVEL=1
```
#### Start Stunnel
```
sudo systemctl start stunnel.service
```
#### Enable Stunnel
```
sudo systemctl start stunnel.service
```
#### MASQUERADE Iptable
```
iptables -t nat -A POSTROUTING -j MASQUERADE
```
#### reset command for Stunnel if needed
```
service stunnel restart
```
#### status of stunnel if needed
```
service stunnel status
```
