# Домашнее задание к занятию "`Система мониторинга Zabbix`" - `Серкебаев Ринат`





### Задание 1 

Установка ZABBIX 7.4 Ubuntu Server, Frontend, PostgreSQL, APACHE

```
sudo apt install postgresql
sudo systemctl is-active postgresql

sudo -s

wget https://repo.zabbix.com/zabbix/7.4/release/ubuntu/pool/main/z/zabbix-release/zabbix-release\_latest\_7.4+ubuntu24.04\_all.deb
dpkg -i zabbix-release\_latest\_7.4+ubuntu24.04\_all.deb
apt update
apt install zabbix-server-pgsql zabbix-frontend-php php8.3-pgsql zabbix-apache-conf zabbix-sql-scripts

sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix

zcat /usr/share/zabbix/sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix

sudo sed -i '/# DBPassword=/a DBPassword=123456789' /etc/zabbix/zabbix\_server.conf

systemctl restart zabbix-server apache2
systemctl enable zabbix-server apache2

http://192.168.1.116/zabbix/zabbix.php?action=item.list\&context=host\&filter\_set=1\&filter\_hostids%5B0%5D=10770
```

скриншот авторизации в админке
<img src = "img/webui.png" width = 100%>

главная страница
<img src = "img/mainpage.png" width = 100%>

---

### Задание 2 

скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу
<img src = "img/hosts.png" width = 100%>

скриншот лога zabbix agent, где видно, что он работает с сервером
<img src = "img/agentlog.png" width = 100%>

скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.
<img src = "img/monitoring.png" width = 100%>



текст использованных команд в GitHub

```
apt install zabbix-agent

sudo sed -i 's/ServerActive=127.0.0.1/ServerActive=192.168.1.116/g' /etc/zabbix/zabbix\_agentd.conf
sudo sed -i 's/Server=127.0.0.1/Server=192.168.1.116/g' /etc/zabbix/zabbix\_agentd.conf

sudo systemctl restart zabbix-agent
sudo systemctl enable zabbix-agent

```

