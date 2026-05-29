
- ==**Tải gói release:**== 
sudo wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_7.0-2+ubuntu24.04_all.deb
- ==**Cài và cập nhật repo:**==
sudo dpkg -i zabbix-release_7.0-2+ubuntu24.04_all.deb 
sudo apt update
- ==**Cài Zabbix Proxy với SQLite3**==
sudo apt install zabbix-proxy-sqlite3 -y
sudo apt install zabbix-sql-scripts -y
- ==**Tạo thư mục DB**==
sudo mkdir -p /var/lib/zabbix
sudo chown zabbix:zabbix /var/lib/zabbix 
- ==Tạo db mới và **Import schema vào DB mới==**
sudo -u zabbix sqlite3 /var/lib/zabbix/zabbix_proxy.db < /usr/share/zabbix-sql-scripts/sqlite3/proxy.sql
- ==**Cấp quyền cho user zabbix**==
sudo chown zabbix:zabbix /var/lib/zabbix/zabbix_proxy.db 
sudo chmod 640 /var/lib/zabbix/zabbix_proxy.db
- ==**Kiểm tra DB có bảng chưa (phải thấy danh sách bảng)**==
sqlite3 /var/lib/zabbix/zabbix_proxy.db ".tables"
- ==**Cấu hình `/etc/zabbix/zabbix_proxy.conf` :**==
sudo nano /etc/zabbix/zabbix_proxy.conf

 **/ Tham số bắt buộc / :**
1. ProxyMode : 0
2. Server=IP server
3. Hostname=name proxy trên web
4. DBName=/var/log/zabbix/zabbix_proxy.log
5. LogFile=/var/log/zabbix/zabbix_proxy.log
6. ConfigFrequency=60
7. DataSenderFrequency=5
8. Timeout=4
9. StatsAllowedIP=127.0.0.1,IP Server

