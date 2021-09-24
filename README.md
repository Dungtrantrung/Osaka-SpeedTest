### Đăng ký tài khoản ookla
- Đăng ký tài khoản tại địa chỉ sau:
https://account.ookla.com/register/servers?step=1
### Cài đặt server 
#### Cài đặt EPEL
```
yum -y install epel-release
yum -y install wget
yum update -y
```
#### Cấu hình NTP
Cài đặt chrony
```
timedatectl set-timezone Asia/Ho_Chi_Minh
yum -y install chrony
NTP_SERVER_IP='0.vn.pool.ntp.org'
sed -i '/server/d' /etc/chrony.conf
echo "server $NTP_SERVER_IP iburst" >> /etc/chrony.conf
systemctl enable chronyd.service
systemctl restart chronyd.service
chronyc sources
```
#### Cài đặt ookla
```
wget https://install.speedtest.net/ooklaserver/ooklaserver.sh
chmod a+x ooklaserver.sh
./ooklaserver.sh install
./ooklaserver.sh start
```
#### Cấu hình LetsEncrypt
- Installing snapd
```
 sudo yum install snapd
 sudo systemctl enable --now snapd.socket
 sudo ln -s /var/lib/snapd/snap /snap
```
- Cài đặt cerbot
```
sudo snap install core; sudo snap refresh core
sudo snap install --classic certbot
```
- Tạo Cert 
```
sudo certbot certonly --standalone
```
Làm theo các hướng dẫn để khởi tạo cert
#### Cấu hình SSL cho Ookla
- Chỉnh sửa file OoklaServer.properties
```
vi OoklaServer.properties
```
Uncomment các dòng sau:
```
OoklaServer.enableAutoUpdate = true
OoklaServer.ssl.useLetsEncrypt = true
openSSL.server.certificateFile = /etc/letsencrypt/live/domain.com/fullchain.pem
openSSL.server.privateKeyFile = /etc/letsencrypt/live/domain.com/privkey.pem
```
Restart lại server Ookla
```
./ooklaserver.sh restart
```
Để kiểm tra và troubleshoot server truy cập vào site: https://www.ookla.com/host-tester
Kết quả: 
![image](https://user-images.githubusercontent.com/11348405/132934408-48ce434c-f44f-4895-b873-aa2624a9c39c.png)
### Đăng ký server với ookla
Sau khi đăng ký tài khoản và đăng nhập thì tiến hành add server điền các thông tin như bên dưới
![image](https://user-images.githubusercontent.com/11348405/132934477-8b5db8c9-5632-46e4-8244-d6ba19fb7040.png)
Sau khi điền xong thông tin thì đợi ookla approve


