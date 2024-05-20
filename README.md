# Jarkom-Modul-3-IT20-2024
Laporan Resmi Praktikum Jaringan Komputer Modul 3

**Kelompok IT20**
| Nama | NRP |
|-----------------------|--------------|
| Clara Valentina | 5027221016 |
| Muhammad Arsy Athallah| 5027221048 |

### Konfigurasi Awal
#### Prefix IP IT20
`192.243`
#### Konfigurasi Nodes
###### Arakis (Router(DHCP Relay))
```
# DHCP config for eth0
auto eth0
iface eth0 inet dhcp


# Static config for eth1
auto eth1
iface eth1 inet static
	address 192.243.1.1
	netmask 255.255.255.0

# Static config for eth2
auto eth2
iface eth2 inet static
	address 192.243.2.1
	netmask 255.255.255.0

# Static config for eth3
auto eth3
iface eth3 inet static
	address 192.243.3.1
	netmask 255.255.255.0

# Static config for eth4
auto eth4
iface eth4 inet static
	address 192.243.4.1
	netmask 255.255.255.0
```
###### Client (Dmitri, Paul)
```
auto eth0
iface eth0 inet dhcp
```
##### Harkonen (Switch 1)
###### Vladimir (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 192.243.1.2
	netmask 255.255.255.0
	gateway 192.243.1.1
```
###### Rabban (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 192.243.1.3
	netmask 255.255.255.0
	gateway 192.243.1.1
```
###### Feyd (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 192.243.1.4
	netmask 255.255.255.0
	gateway 192.243.1.1
```
##### Atreides (Switch 2)
###### Leto (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 192.243.2.2
	netmask 255.255.255.0
	gateway 192.243.2.1
```
###### Duncan (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 192.243.2.3
	netmask 255.255.255.0
	gateway 192.243.2.1
```
###### Jessica (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 192.243.2.4
	netmask 255.255.255.0
	gateway 192.243.2.1
```
#####  Corrino (Switch 3)
###### Irulan (DNS Server)
```
auto eth0
iface eth0 inet static
	address 192.243.3.2
	netmask 255.255.255.0
	gateway 192.243.3.1
```
###### Mohiam (DHCP Server)
```
auto eth0
iface eth0 inet static
	address 192.243.3.3
	netmask 255.255.255.0
	gateway 192.243.3.1
```
##### Fremen (Switch 4)
###### Chani (Database Server)
```
auto eth0
iface eth0 inet static
	address 192.243.4.2
	netmask 255.255.255.0
	gateway 192.243.4.1
```
###### Stilgar (Load Balancer)
```
auto eth0
iface eth0 inet static
	address 192.243.4.3
	netmask 255.255.255.0
	gateway 192.243.4.1 
```
#### List IP 
| Node    | Kategori             | IP Address     |
|---------|----------------------|----------------|
| Mohiam  | DHCP Server          | 192.243.3.3    |
| Irulan  | DNS Server           | 192.243.3.2    |
| Chani   | Database Server      | 192.243.4.2    |
| Stilgar | Load Balancer        | 192.243.4.3    |
| Leto    | Laravel Worker       | 192.243.2.2    |
| Duncan  | Laravel Worker       | 192.243.2.3    |
| Jessica | Laravel Worker       | 192.243.2.4    |
| Vladimir| PHP Worker           | 192.243.1.2    |
| Rabban  | PHP Worker           | 192.243.1.3    |
| Feyd    | PHP Worker           | 192.243.1.4    |

**No 6**

Vladimir Harkonen memerintahkan setiap worker(harkonen) PHP, untuk melakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3.

Pertama buat script phpworker.sh di Vladimir, Rabban, dan Feyd.

```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf


apt-get update
apt-get install nginx wget unzip php7.3 php-fpm -y


service nginx start


wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1lmnXJUbyx1JDt2OA5z_1dEowxozfkn30' -O 'harkonen.zip'
unzip harkonen.zip


# meletakkan komponen web
mkdir -p /var/www/jarkomit20
mv /root/modul-3/* /var/www/jarkomit20/


rm -rf /root/modul-3
rm harkonen.zip


echo 'server {
    listen 80;


    server_name _;


    root /var/www/jarkomit20;
    index index.php index.html index.htm;
   
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }


    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
    }


    error_log /var/log/nginx/jarkomit20_error.log;
    access_log /var/log/nginx/jarkomit20_access.log;
}
' > /etc/nginx/sites-available/jarkomit20.conf


rm /etc/nginx/sites-enabled/default
ln -s /etc/nginx/sites-available/jarkomit20.conf /etc/nginx/sites-enabled/


service nginx restart
service php7.3-fpm start
```

