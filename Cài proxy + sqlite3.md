B1: Tải gói release:
sudo wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_7.0-2+ubuntu22.04_all.deb

B2: Cài và cập nhật repo:
sudo dpkg -i zabbix-release_7.0-2+ubuntu22.04_all.deb
sudo apt update

B3: Cài Zabbix Proxy với SQLite3:
sudo apt install zabbix-proxy-sqlite3 -y
sudo apt install zabbix-sql-scripts -y

B4: Tạo thư mục DB:
sudo mkdir -p /var/lib/zabbix
sudo chown zabbix:zabbix /var/lib/zabbix

B5: Tạo DB và Import schema:
sudo -u zabbix sqlite3 /var/lib/zabbix/zabbix_proxy.db < /usr/share/zabbix-sql-scripts/sqlite3/proxy.sql

B6: Cấp quyền cho user zabbix:
sudo chown zabbix:zabbix /var/lib/zabbix/zabbix_proxy.db
sudo chmod 640 /var/lib/zabbix/zabbix_proxy.db

B7: Kiểm tra DB
sqlite3 /var/lib/zabbix/zabbix_proxy.db ".tables"

B8: Cấu hình
sudo nano /etc/zabbix/zabbix_proxy.conf

- Các tham số cần sửa:
ProxyMode=0
Server=<IP_Server>
Hostname=<Tên_proxy_trên_web>
DBName=/var/lib/zabbix/zabbix_proxy.db
LogFile=/var/log/zabbix/zabbix_proxy.log
ConfigFrequency=60
DataSenderFrequency=5
Timeout=4
StatsAllowedIP=127.0.0.1,<IP_Server>