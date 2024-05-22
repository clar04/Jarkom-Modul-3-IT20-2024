# Jarkom-Modul-3-IT20-2024
Laporan Resmi Praktikum Jaringan Komputer Modul 3

**Kelompok IT20**
| Nama | NRP |
|-----------------------|--------------|
| Clara Valentina | 5027221016 |
| Muhammad Arsy Athallah| 5027221048 |

### Topologi

<img width="999" alt="image" src="https://github.com/clar04/Jarkom-Modul-3-IT20-2024/assets/123356941/7879570e-e796-4385-b5c2-ae98b87be7e5">

## Soal 0 - 1
Planet Caladan sedang mengalami krisis karena kehabisan spice, klan atreides berencana untuk melakukan eksplorasi ke planet arakis dipimpin oleh duke leto mereka meregister domain name atreides.yyy.com untuk worker Laravel mengarah pada Leto Atreides . Namun ternyata tidak hanya klan atreides yang berusaha melakukan eksplorasi, Klan harkonen sudah mendaftarkan domain name harkonen.yyy.com untuk worker PHP (0) mengarah pada Vladimir Harkonen.
Jalankan script berikut di `Irulan`
```
echo 'zone "atreides.it20.com" {
        type master;
        file "/etc/bind/jarkom/atreides.it20.com";
};


zone "harkonen.it20.com" {
        type master;
        file "/etc/bind/jarkom/harkonen.it20.com";
};' > /etc/bind/named.conf.local


mkdir /etc/bind/jarkom


echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     atreides.it20.com. root.atreides.it20.com. (
                        2024051601      ; Serial
                        604800          ; Refresh
                        86400           ; Retry
                        2419200         ; Expire
                        604800 )        ; Negative Cache TTL
;
@               IN      NS      atreides.it20.com.
@               IN      A       192.243.2.2; IP Leto Laravel Worker' > /etc/bind/jarkom/atreides.it20.com


echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     harkonen.it20.com.  harkonen.it20.com.  (
                        2024051601      ; Serial
                        604800          ; Refresh
                        86400           ; Retry
                        2419200         ; Expire
                        604800 )        ; Negative Cache TTL
;
@               IN      NS      harkonen.it20.com.
@               IN      A       192.243.1.2 ; IP Vladimir PHP Worker' > /etc/bind/jarkom/harkonen.it20.com


echo 'options {
        directory "/var/cache/bind";


        forwarders {
                192.168.122.1;
        };


        // dnssec-validation auto;
        allow-query{any;};
        auth-nxdomain no;    
        listen-on-v6 { any; };
}; ' >/etc/bind/named.conf.options


service bind9 restart
```
(1) Lakukan konfigurasi sesuai dengan peta yang sudah diberikan.

#### Konfigurasi Awal
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

#### Setup Node
**Arakis**
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.243.0.0/16

apt-get update
apt-get install isc-dhcp-relay -y
service isc-dhcp-relay start

echo '
# IP DHCP Server -> IP Mohiam
SERVERS="192.243.3.3"
# Interfaces to listen on
INTERFACES="eth1 eth2 eth3 eth4"
# Options to pass to the DHCP relay
OPTIONS=""
' > /etc/default/isc-dhcp-relay

echo net.ipv4.ip_forward=1 > /etc/sysctl.conf

service isc-dhcp-relay restart

```

**Irulan**
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf

# Install necessary packages
apt-get update
apt-get install -y bind9

echo '
options {
      directory "/var/cache/bind";

      forwarders {
          192.168.122.1;
      };

      // dnssec-validation auto;
      allow-query{ any; };
      auth-nxdomain no;    # conform to RFC1035
      listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

service bind9 restart
```

**Mohiam**
```
echo 'nameserver 192.243.3.2' > /etc/resolv.conf

apt-get update
apt-get install isc-dhcp-server
service isc-dhcp-server status

echo 'INTERFACESv4="eth0"' > /etc/default/isc-dhcp-server

echo '
ddns-update-style none;

# Subnet Switch Harkonen
subnet 192.248.1.0 netmask 255.255.255.0 {
}

# Subnet Switch Atreides
subnet 192.248.2.0 netmask 255.255.255.0 {
}

subnet 192.248.3.0 netmask 255.255.255.0 {
}

subnet 192.248.4.0 netmask 255.255.255.0 {
}' > /etc/dhcp/dhcpd.conf


service isc-dhcp-server restart
```

**Chani**
```
echo 'nameserver 192.243.3.2' > /etc/resolv.conf
apt-get update
apt-get install mariadb-server -y
service mysql start
```

**Stilgar**
```
echo 'nameserver 192.243.3.2' > /etc/resolv.conf
apt-get update
apt-get install apache2-utils -y
apt-get install nginx -y
apt-get install lynx -y

service nginx start
```

**PHP Worker**
```
echo 'nameserver 192.243.3.2' > /etc/resolv.conf
apt-get update
apt-get install nginx -y
apt-get install wget -y
apt-get install unzip -y
apt-get install lynx -y
apt-get install htop -y
apt-get install apache2-utils -y
apt-get install php7.3-fpm php7.3-common php7.3-mysql php7.3-gmp php7.3-curl php7.3-intl php7.3-mbstring php7.3-xmlrpc php7.3-gd php7.3-xml php7.3-cli php7.3-zip -y

service nginx start
service php7.3-fpm start
```

**Lavarel Worker**
```
apt-get update

apt-get install mariadb-client -y

apt-get install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2

curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg

sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'

apt-get update

apt-get install php8.0-mbstring php8.0-xml php8.0-cli php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget -y
apt-get install nginx -y

wget https://getcomposer.org/download/2.0.13/composer.phar
chmod +x composer.phar
mv composer.phar /usr/bin/composer

apt-get install git -y

apt-get install lynx
```

**Client**
```
apt update
apt install lynx -y
apt install htop -y
apt install apache2-utils -y
apt-get install jq -y
```
#### Dokumentasi ping harkonen.it20.com dan atreides.it20.com pada client Dmitri

<img width="524" alt="image" src="https://github.com/clar04/Jarkom-Modul-3-IT20-2024/assets/123356941/b12c1e79-47d9-4969-a620-a7e8ef5cacda">

#### Dokumentasi ping harkonen.it20.com dan atreides.it20.com pada client Paul

<img width="547" alt="image" src="https://github.com/clar04/Jarkom-Modul-3-IT20-2024/assets/123356941/a42ce5a9-16bb-4042-857c-352dee92a908">

## Soal 2
Kemudian, karena masih banyak spice yang harus dikumpulkan, bantulah para aterides untuk bersaing dengan harkonen dengan kriteria berikut.:
1. Semua CLIENT harus menggunakan konfigurasi dari DHCP Server.
2. Client yang melalui House Harkonen mendapatkan range IP dari [prefix IP].1.14 - [prefix IP].1.28 dan [prefix IP].1.49 - [prefix IP].1.70 (2)

Pada Irulan, menjalankan script berikut 
```
echo 'subnet 192.243.1.1 netmask 255.255.255.0 {
    range 192.243.1.14 192.243.1.28;
    range 192.243.1.49 192.243.1.70;
    option routers 192.243.1.1;
}

subnet 192.243.2.1 netmask 255.255.255.0 {
}

subnet 192.243.3.0 netmask 255.255.255.0 {
}

subnet 192.243.4.0 netmask 255.255.255.0 {

}' > /etc/dhcp/dhcpd.conf

service bind9 start
```

## Soal 3
3. Client yang melalui House Atreides mendapatkan range IP dari [prefix IP].2.15 - [prefix IP].2.25 dan [prefix IP].2 .200 - [prefix IP].2.210 (3)
Pada Mohiam menjalankan script berikut
```
echo 'subnet 192.243.1.1 netmask 255.255.255.0 {
    range 192.243.1.14 192.243.1.28;
    range 192.243.1.49 192.243.1.70;
    option routers 192.243.1.1;
}

subnet 192.243.2.1 netmask 255.255.255.0 {
    range 192.243.2.15 192.243.2.25;
    range 192.243.2.200 192.243.2.210;
    option routers 192.243.2.1;
}

subnet 192.243.3.0 netmask 255.255.255.0 {
}

subnet 192.243.4.0 netmask 255.255.255.0 {


}' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server start

```

## Soal 4
4. Client mendapatkan DNS dari Princess Irulan dan dapat terhubung dengan internet melalui DNS tersebut (4)
Pada Mohiam menjalankan script berikut
```
echo 'subnet 192.243.1.0 netmask 255.255.255.0 {
    range 192.243.1.14 192.243.1.28;
    range 192.243.1.49 192.243.1.70;
    option routers 192.243.1.1;
    option broadcast-address 192.243.1.255;
    option domain-name-servers 192.243.3.2;
}

subnet 192.243.2.0 netmask 255.255.255.0 {
    range 192.243.2.15 192.243.2.25;
    range 192.243.2.200 192.243.2.210;
    option routers 192.243.2.1;
    option broadcast-address 192.243.2.255;
    option domain-name-servers 192.243.3.2;
}

subnet 192.243.3.0 netmask 255.255.255.0 {
}

subnet 192.243.4.0 netmask 255.255.255.0 {
}' > /etc/dhcp/dhcpd.conf



service isc-dhcp-server start

```

## Soal 5
Durasi DHCP server meminjamkan alamat IP kepada Client yang melalui House Harkonen selama 5 menit sedangkan pada client yang melalui House Atreides selama 20 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 87 menit. (5)
```
echo 'subnet 192.243.1.1 netmask 255.255.255.0 {
    range 192.243.1.14 192.243.1.28;
    range 192.243.1.49 192.243.1.70;
    option routers 192.243.1.1;
    option broadcast-address 192.243.1.255;
    option domain-name-servers 192.243.3.2;
    default-lease-time 300;
    max-lease-time 5220;
}

subnet 192.243.2.1 netmask 255.255.255.0 {
    range 192.243.2.15 192.243.2.25;
    range 192.243.2.200 192.243.2.210;
    option routers 192.243.2.1;
    option broadcast-address 192.243.2.255;
    option domain-name-servers 192.243.3.2;
    default-lease-time 1200;
    max-lease-time 5220;
}

subnet 192.243.3.0 netmask 255.255.255.0 {
}

subnet 192.243.4.0 netmask 255.255.255.0 {

}' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server start
```
#### Dokumentasi pada client Dmitri untuk no 2, 4, 5
<img width="442" alt="image" src="https://github.com/clar04/Jarkom-Modul-3-IT20-2024/assets/123356941/0543e511-6bf5-4044-90c6-5e57d43cbe1f">

#### Dokumentasi pada client Paul untuk no 3, 4, 5
<img width="420" alt="image" src="https://github.com/clar04/Jarkom-Modul-3-IT20-2024/assets/123356941/c66c9c2f-5992-485b-b09b-08e9bb2bd159">


## Soal 6
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

## Soal 7
Aturlah agar Stilgar dari fremen dapat dapat bekerja sama dengan maksimal, lalu lakukan testing dengan 5000 request dan 150 request/second.(7)
Masuk ke stilgar dan buat .sh baru
~~~
#!/bin/bash
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt update
apt install nginx php php-fpm lynx apache2-utils -y

echo 'upstream round_robin  {
    server 192.243.1.2 ; #IP Vladimir
    server 192.243.1.3 ; #IP Rabban
    server 192.243.1.4 ; #IP Feyd
}

server {
    listen 8080;
        

        location / {
            proxy_pass http://round_robin;
            proxy_set_header    X-Real-IP $remote_addr;
            proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header    Host $http_host;
        }

    error_log /var/log/nginx/lb_error.log;
    access_log /var/log/nginx/lb_access.log;
}' >/etc/nginx/sites-available/round-robin

unlink /etc/nginx/sites-enabled/default

ln -s /etc/nginx/sites-available/round-robin /etc/nginx/sites-enabled/round-robin

service nginx restart
~~~
Setelah itu masuk ke client dan jalankan command ab -n 5000 -c 150 http://192.243.4.3:8080/
![image](https://github.com/clar04/Jarkom-Modul-3-IT20-2024/assets/128389289/faf809c8-fcae-4bc6-bd78-4f12885dcedd)

## Soal 8
Karena diminta untuk menuliskan peta tercepat menuju spice, buatlah analisis hasil testing dengan 500 request dan 50 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
a. Nama Algoritma Load Balancer
b. Report hasil testing pada Apache Benchmark
c. Grafik request per second untuk masing masing algoritma. 
d. Analisis(8)
buat .sh baru di stilgar dengan scripting seperti ini:
~~~
echo 'upstream round_robin  {
    server 192.243.1.2 ; #IP Vladimir
    server 192.243.1.3 ; #IP Rabban
    server 192.243.1.4 ; #IP Feyd
}

server {
    listen 8080;
        

        location / {
            proxy_pass http://round_robin;
            proxy_set_header    X-Real-IP $remote_addr;
            proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header    Host $http_host;
        }

    error_log /var/log/nginx/lb_error.log;
    access_log /var/log/nginx/lb_access.log;
}' >/etc/nginx/sites-available/round-robin

echo 'upstream generic_hash  {
    hash $request_uri consistent;
    server 192.243.1.2 ; #IP Vladimir
    server 192.243.1.3 ; #IP Rabban
    server 192.243.1.4 ; #IP Feyd
}

server {
    listen 8081;

        location / {
            proxy_pass http://generic_hash;
            proxy_set_header    X-Real-IP $remote_addr;
            proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header    Host $http_host;
        }

    error_log /var/log/nginx/lb_error.log;
    access_log /var/log/nginx/lb_access.log;
}' >/etc/nginx/sites-available/generic-hash

echo 'upstream ip_hash  {
    ip_hash;
    server 192.243.1.2 ; #IP Vladimir
    server 192.243.1.3 ; #IP Rabban
    server 192.243.1.4 ; #IP Feyd
}

server {
    listen 8082;

        location / {
            proxy_pass http://ip_hash;
            proxy_set_header    X-Real-IP $remote_addr;
            proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header    Host $http_host;
        }

    error_log /var/log/nginx/lb_error.log;
    access_log /var/log/nginx/lb_access.log;
}' >/etc/nginx/sites-available/ip-hash

echo 'upstream least_conn  {
    least_conn;
    server 192.243.1.2 ; #IP Vladimir
    server 192.243.1.3 ; #IP Rabban
    server 192.243.1.4 ; #IP Feyd
}

server {
    listen 8083;
        
        location / {
            proxy_pass http://least_conn;
            proxy_set_header    X-Real-IP $remote_addr;
            proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header    Host $http_host;
        }

    error_log /var/log/nginx/lb_error.log;
    access_log /var/log/nginx/lb_access.log;
}' >/etc/nginx/sites-available/least-conn

unlink /etc/nginx/sites-enabled/default

ln -s /etc/nginx/sites-available/round-robin /etc/nginx/sites-enabled/round-robin
ln -s /etc/nginx/sites-available/generic-hash /etc/nginx/sites-enabled/generic-hash
ln -s /etc/nginx/sites-available/ip-hash /etc/nginx/sites-enabled/ip-hash
ln -s /etc/nginx/sites-available/least-conn /etc/nginx/sites-enabled/least-conn

service nginx restart
~~~
lalu masuk ke client dan buat scripting dengan nama test.sh untuk mendapatkan report:
~~~
#!/bin/bash

LB_ADDR="192.243.4.3"

RR_IP="8080"    # IP Round Robin
IPH_IP="8082"   # IP IP Hash
GENH_IP="8081"  # IP Generic Hash

LC3W_IP="8083"  # IP Least Conn 3 Workers
LC2W_IP="8004"  # IP Least Conn 2 Workers
LC1W_IP="8005"  # IP Least Conn 1 Worker

mkdir -p /root/report

RoundRobinTest() {
  AMT=$1
  CON=$2

  echo "$(ab -n $AMT -c $CON http:\/\/$LB_ADDR:$RR_IP\/)" > /root/report/round_robin_test.bin
  echo "Round Robin Test Done"
}

IPHashTest() {
  AMT=$1
  CON=$2

  echo "$(ab -n $AMT -c $CON http:\/\/$LB_ADDR:$IPH_IP\/)" > /root/report/ip_hash_test.bin
  echo "IP Hash Test Done"
}

GenHashTest() {
  AMT=$1
  CON=$2

  echo "$(ab -n $AMT -c $CON http:\/\/$LB_ADDR:$GENH_IP\/)" > /root/report/gen_hash_test.bin
  echo "Generic Hash Test Done"
}

LeastConn3Test() {
  AMT=$1
  CON=$2

  echo "$(ab -n $AMT -c $CON http:\/\/$LB_ADDR:$LC3W_IP\/)" > /root/report/least_conn_3_test.bin
  echo "Least Connection with 3 Workers Test Done"
}

LeastConn2Test() {
  AMT=$1
  CON=$2

  echo "$(ab -n $AMT -c $CON http:\/\/$LB_ADDR:$LC2W_IP\/)" > /root/report/least_conn_2_test.bin
  echo "Least Connection with 2 Workers Test Done"
}

LeastConn1Test() {
  AMT=$1
  CON=$2

  echo "$(ab -n $AMT -c $CON http:\/\/$LB_ADDR:$LC1W_IP\/)" > /root/report/least_conn_1_test.bin
  echo "Least Connection with 1 Worker Test Done"
}

SingleTest() {
  echo "
========== Single Test ==========

Pilih Load Balancer:

1.) Round Robin - 3 Workers
2.) IP Hash - 3 Workers
3.) Generic Hash - 3 Workers
4.) Least Connection - 3 Workers
5.) Least Connection - 2 Workers
6.) Least Connection - 1 Workers
"

  echo -n "Pilih Opsi Pengujian: "
  read optLB

  if [ $optLB -eq 1 ];
  then
    echo -n "Masukkan Jumlah Request: "
    read inReq
    echo -n "Masukkan Jumlah Concurrency: "
    read inCon

    RoundRobinTest $inReq $inCon

    echo -e "\n\n+++++ Laporan Hasil Pengujian Load Balancer +++++"
    echo -e "\nKeterangan Pengujian"
    echo -e "Nama Load Balancer: \t Round Robin"
    echo -e "Jumlah Worker: \t\t 3 Workers"

    echo -e "\nTabel Hasil\n"
    # Menampilkan header tabel
    echo -e "Load Balancer\tWorker(s)\tReq per second\tComplete req\tFailed req\tTime per req\tTransfer rate"

    # Mengambil data dari file
    awk '/Requests per second/{req_sec=$(NF-2)} /Complete requests/{comp_req=$NF} /Failed requests/{fail_req=$NF} /Time per request/{time_req=$(NF-6)} /Transfer rate/{transfer_rate=$(NF-2)} END {printf "Round Robin\t3\t\t%s\t\t%s\t\t%s\t\t%s\t\t%s\n", req_sec, comp_req, fail_req, time_req, transfer_rate}' /root/report/round_robin_test.bin
    echo -e "\n\n+++++ Akhir Laporan +++++\n"
  elif [ $optLB -eq 2 ]; then
    echo -n "Masukkan Jumlah Request: "
    read inReq
    echo -n "Masukkan Jumlah Concurrency: "
    read inCon

    IPHashTest $inReq $inCon

    echo -e "\n\n+++++ Laporan Hasil Pengujian Load Balancer +++++"
    echo -e "\nKeterangan Pengujian"
    echo -e "Nama Load Balancer: \t IP Hash"
    echo -e "Jumlah Worker: \t\t 3 Workers"

    echo -e "\nTabel Hasil\n"
    # Menampilkan header tabel
    echo -e "Load Balancer\tWorker(s)\tReq per second\tComplete req\tFailed req\tTime per req\tTransfer rate"

    # Mengambil data dari file
    awk '/Requests per second/{req_sec=$(NF-2)} /Complete requests/{comp_req=$NF} /Failed requests/{fail_req=$NF} /Time per request/{time_req=$(NF-6)} /Transfer rate/{transfer_rate=$(NF-2)} END {printf "IP Hash\t\t3\t\t%s\t\t%s\t\t%s\t\t%s\t\t%s\n", req_sec, comp_req, fail_req, time_req, transfer_rate}' /root/report/ip_hash_test.bin
    echo -e "\n\n+++++ Akhir Laporan +++++\n"
  elif [ $optLB -eq 3 ]; then
    echo -n "Masukkan Jumlah Request: "
    read inReq
    echo -n "Masukkan Jumlah Concurrency: "
    read inCon

    GenHashTest $inReq $inCon

    echo -e "\n\n+++++ Laporan Hasil Pengujian Load Balancer +++++"
    echo -e "\nKeterangan Pengujian"
    echo -e "Nama Load Balancer: \t Generic Hash"
    echo -e "Jumlah Worker: \t\t 3 Workers"

    echo -e "\nTabel Hasil\n"
    # Menampilkan header tabel
    echo -e "Load Balancer\tWorker(s)\tReq per second\tComplete req\tFailed req\tTime per req\tTransfer rate"

    # Mengambil data dari file
    awk '/Requests per second/{req_sec=$(NF-2)} /Complete requests/{comp_req=$NF} /Failed requests/{fail_req=$NF} /Time per request/{time_req=$(NF-6)} /Transfer rate/{transfer_rate=$(NF-2)} END {printf "Generic Hash\t3\t\t%s\t\t%s\t\t%s\t\t%s\t\t%s\n", req_sec, comp_req, fail_req, time_req, transfer_rate}' /root/report/gen_hash_test.bin
    echo -e "\n\n+++++ Akhir Laporan +++++\n"
  elif [ $optLB -eq 4 ]; then
    echo -n "Masukkan Jumlah Request: "
    read inReq
    echo -n "Masukkan Jumlah Concurrency: "
    read inCon

    LeastConn3Test $inReq $inCon

    echo -e "\n\n+++++ Laporan Hasil Pengujian Load Balancer +++++"
    echo -e "\nKeterangan Pengujian"
    echo -e "Nama Load Balancer: \t Least Connection"
    echo -e "Jumlah Worker: \t\t 3 Workers"

    echo -e "\nTabel Hasil\n"
    # Menampilkan header tabel
    echo -e "Load Balancer\tWorker(s)\tReq per second\tComplete req\tFailed req\tTime per req\tTransfer rate"

    # Mengambil data dari file
    awk '/Requests per second/{req_sec=$(NF-2)} /Complete requests/{comp_req=$NF} /Failed requests/{fail_req=$NF} /Time per request/{time_req=$(NF-6)} /Transfer rate/{transfer_rate=$(NF-2)} END {printf "Least Conn\t3\t\t%s\t\t%s\t\t%s\t\t%s\t\t%s\n", req_sec, comp_req, fail_req, time_req, transfer_rate}' /root/report/least_conn_3_test.bin
    echo -e "\n\n+++++ Akhir Laporan +++++\n"
  elif [ $optLB -eq 5 ]; then
    echo -n "Masukkan Jumlah Request: "
    read inReq
    echo -n "Masukkan Jumlah Concurrency: "
    read inCon

    LeastConn2Test $inReq $inCon

    echo -e "\n\n+++++ Laporan Hasil Pengujian Load Balancer +++++"
    echo -e "\nKeterangan Pengujian"
    echo -e "Nama Load Balancer: \t Least Connection"
    echo -e "Jumlah Worker: \t\t 2 Workers"

    echo -e "\nTabel Hasil\n"
    # Menampilkan header tabel
    echo -e "Load Balancer\tWorker(s)\tReq per second\tComplete req\tFailed req\tTime per req\tTransfer rate"

    # Mengambil data dari file
    awk '/Requests per second/{req_sec=$(NF-2)} /Complete requests/{comp_req=$NF} /Failed requests/{fail_req=$NF} /Time per request/{time_req=$(NF-6)} /Transfer rate/{transfer_rate=$(NF-2)} END {printf "Least Conn\t2\t\t%s\t\t%s\t\t%s\t\t%s\t\t%s\n", req_sec, comp_req, fail_req, time_req, transfer_rate}' /root/report/least_conn_2_test.bin
    echo -e "\n\n+++++ Akhir Laporan +++++\n"
  elif [ $optLB -eq 6 ]; then
    echo -n "Masukkan Jumlah Request: "
    read inReq
    echo -n "Masukkan Jumlah Concurrency: "
    read inCon

    LeastConn1Test $inReq $inCon

    echo -e "\n\n+++++ Laporan Hasil Pengujian Load Balancer +++++"
    echo -e "\nKeterangan Pengujian"
    echo -e "Nama Load Balancer: \t LEast Connection"
    echo -e "Jumlah Worker: \t\t 1 Workers"

    echo -e "\nTabel Hasil\n"
    # Menampilkan header tabel
    echo -e "Load Balancer\tWorker(s)\tReq per second\tComplete req\tFailed req\tTime per req\tTransfer rate"

    # Mengambil data dari file
    awk '/Requests per second/{req_sec=$(NF-2)} /Complete requests/{comp_req=$NF} /Failed requests/{fail_req=$NF} /Time per request/{time_req=$(NF-6)} /Transfer rate/{transfer_rate=$(NF-2)} END {printf "Least Conn\t1\t\t%s\t\t%s\t\t%s\t\t%s\t\t%s\n", req_sec, comp_req, fail_req, time_req, transfer_rate}' /root/report/least_conn_1_test.bin
    echo -e "\n\n+++++ Akhir Laporan +++++\n"
  else
    echo "Unknown Input"
  fi
}

MultiTestLB() {
  echo "
========== Multi Test - By LB Method ==========

"
  echo -n "Masukkan Jumlah Request: "
  read inReq
  echo -n "Masukkan Jumlah Concurrency: "
  read inCon

  RoundRobinTest $inReq $inCon
  sleep 1
  IPHashTest $inReq $inCon
  sleep 1
  GenHashTest $inReq $inCon
  sleep 1
  LeastConn3Test $inReq $inCon

  echo "Multi Test Done"

  echo -e "\n\n+++++ Laporan Hasil Pengujian Load Balancer +++++"
  echo -e "\nKeterangan Pengujian"
  echo -e "Nama Load Balancer: \t Round Robin, Least Connection, IP Hash, dan Generic Hash"
  echo -e "Jumlah Worker: \t\t 3 Workers"

  echo -e "\nTabel Hasil\n"
  # Menampilkan header tabel
  echo -e "Load Balancer\tWorker(s)\tReq per second\tComplete req\tFailed req\tTime per req\tTransfer rate"

  # Mengambil data dari file
  awk '/Requests per second/{req_sec=$(NF-2)} /Complete requests/{comp_req=$NF} /Failed requests/{fail_req=$NF} /Time per request/{time_req=$(NF-6)} /Transfer rate/{transfer_rate=$(NF-2)} END {printf "Round Robin\t3\t\t%s\t\t%s\t\t%s\t\t%s\t\t%s\n", req_sec, comp_req, fail_req, time_req, transfer_rate}' /root/report/round_robin_test.bin
  awk '/Requests per second/{req_sec=$(NF-2)} /Complete requests/{comp_req=$NF} /Failed requests/{fail_req=$NF} /Time per request/{time_req=$(NF-6)} /Transfer rate/{transfer_rate=$(NF-2)} END {printf "Least Conn\t3\t\t%s\t\t%s\t\t%s\t\t%s\t\t%s\n", req_sec, comp_req, fail_req, time_req, transfer_rate}' /root/report/least_conn_3_test.bin
  awk '/Requests per second/{req_sec=$(NF-2)} /Complete requests/{comp_req=$NF} /Failed requests/{fail_req=$NF} /Time per request/{time_req=$(NF-6)} /Transfer rate/{transfer_rate=$(NF-2)} END {printf "IP Hash\t\t3\t\t%s\t\t%s\t\t%s\t\t%s\t\t%s\n", req_sec, comp_req, fail_req, time_req, transfer_rate}' /root/report/ip_hash_test.bin
  awk '/Requests per second/{req_sec=$(NF-2)} /Complete requests/{comp_req=$NF} /Failed requests/{fail_req=$NF} /Time per request/{time_req=$(NF-6)} /Transfer rate/{transfer_rate=$(NF-2)} END {printf "Generic Hash\t3\t\t%s\t\t%s\t\t%s\t\t%s\t\t%s\n", req_sec, comp_req, fail_req, time_req, transfer_rate}' /root/report/gen_hash_test.bin
  echo -e "\n\n+++++ Akhir Laporan +++++\n"
}

MultiTestWorker() {
  echo "
========== Multi Test - By Worker Amount ==========

"

  echo -n "Masukkan Jumlah Request: "
  read inReq
  echo -n "Masukkan Jumlah Concurrency: "
  read inCon

  LeastConn3Test $inReq $inCon
  sleep 1
  LeastConn2Test $inReq $inCon
  sleep 1
  LeastConn1Test $inReq $inCon
  sleep 1
  
  echo "Multi Test Done"

  echo -e "\n\n+++++ Laporan Hasil Pengujian Load Balancer +++++"
  echo -e "\nKeterangan Pengujian"
  echo -e "Nama Load Balancer: \t Round Robin, Least Connection, IP Hash, dan Generic Hash"
  echo -e "Jumlah Worker: \t\t 3 Workers"

  echo -e "\nTabel Hasil\n"
  # Menampilkan header tabel
  echo -e "Load Balancer\tWorker(s)\tReq per second\tComplete req\tFailed req\tTime per req\tTransfer rate"

  # Mengambil data dari file
  awk '/Requests per second/{req_sec=$(NF-2)} /Complete requests/{comp_req=$NF} /Failed requests/{fail_req=$NF} /Time per request/{time_req=$(NF-6)} /Transfer rate/{transfer_rate=$(NF-2)} END {printf "Least Conn\t3\t\t%s\t\t%s\t\t%s\t\t%s\t\t%s\n", req_sec, comp_req, fail_req, time_req, transfer_rate}' /root/report/least_conn_3_test.bin
  awk '/Requests per second/{req_sec=$(NF-2)} /Complete requests/{comp_req=$NF} /Failed requests/{fail_req=$NF} /Time per request/{time_req=$(NF-6)} /Transfer rate/{transfer_rate=$(NF-2)} END {printf "Least Conn\t2\t\t%s\t\t%s\t\t%s\t\t%s\t\t%s\n", req_sec, comp_req, fail_req, time_req, transfer_rate}' /root/report/least_conn_2_test.bin
  awk '/Requests per second/{req_sec=$(NF-2)} /Complete requests/{comp_req=$NF} /Failed requests/{fail_req=$NF} /Time per request/{time_req=$(NF-6)} /Transfer rate/{transfer_rate=$(NF-2)} END {printf "Least Conn\t1\t\t%s\t\t%s\t\t%s\t\t%s\t\t%s\n", req_sec, comp_req, fail_req, time_req, transfer_rate}' /root/report/least_conn_1_test.bin
  echo -e "\n\n+++++ Akhir Laporan +++++\n"
}

echo "
========== Apache Benchmarking ==========
             By Jarkom-IT07

Opsi Pengujian Load Balancer:

1.) Single Test
2.) Multi Test - By LB Method
3.) Multi Test - By Worker Amount

"

echo -n "Pilih Opsi Pengujian: "
read optLB

if [ $optLB -eq 1 ];
then
  SingleTest
elif [ $optLB -eq 2 ]; then
  MultiTestLB
elif [ $optLB -eq 3 ]; then
  MultiTestWorker
else
  echo "Unknown Input"
fi
~~~
Setelah di bash maka akan muncul data dan membuat analisis serta grafik batang dari setiap request per second

## Soal 9
Dengan menggunakan algoritma Least-Connection, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 1000 request dengan 10 request/second, kemudian tambahkan grafiknya pada peta. (9)
Buat .sh baru di stilgar dengan script seperti ini:
~~~
echo 'upstream least_conn_2_worker  {
    least_conn;
    server 192.243.1.2 ; #IP Vladimir
    server 192.243.1.3 ; #IP Rabban
}

server {
    listen 8084;
        
        location / {
            proxy_pass http://least_conn_2_worker;
            proxy_set_header    X-Real-IP $remote_addr;
            proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header    Host $http_host;
        }

    error_log /var/log/nginx/lb_error.log;
    access_log /var/log/nginx/lb_access.log;
}' >/etc/nginx/sites-available/least-conn-2-worker


echo 'upstream least_conn_1_worker  {
    least_conn;
    server 192.243.1.2 ; #IP Vladimir
}

server {
    listen 8085;
        
        location / {
            proxy_pass http://least_conn_1_worker;
            proxy_set_header    X-Real-IP $remote_addr;
            proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header    Host $http_host;
        }

    error_log /var/log/nginx/lb_error.log;
    access_log /var/log/nginx/lb_access.log;
}' >/etc/nginx/sites-available/least-conn-1-worker

ln -s /etc/nginx/sites-available/least-conn-2-worker /etc/nginx/sites-enabled/least-conn-2-worker
ln -s /etc/nginx/sites-available/least-conn-1-worker /etc/nginx/sites-enabled/least-conn-1-worker

service nginx restart
~~~
Setelah itu jalankan test.sh di client dan buat laporan dari request per second sesuai yang diminta oleh soal no 9
## Soal 10
Selanjutnya coba tambahkan keamanan dengan konfigurasi autentikasi di LB dengan dengan kombinasi username: “secmart” dan password: “kcksyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/supersecret/ (10)
buat .sh baru di stilgar dengan script seperti berikut:
~~~
mkdir /etc/nginx/supersecret/

htpasswd -bc /etc/nginx/supersecret/htpasswd secmart kcksit20
echo 'upstream round_robin  {
    server 192.243.1.2 ; #IP Vladimir
    server 192.243.1.3 ; #IP Rabban
    server 192.243.1.4 ; #IP Feyd
}

server {
    listen 8080;
        

        location / {
            auth_basic "Restricted Content";
            auth_basic_user_file /etc/nginx/supersecret/htpasswd;
            proxy_pass http://round_robin;
            proxy_set_header    X-Real-IP $remote_addr;
            proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header    Host $http_host;
        }

    error_log /var/log/nginx/lb_error.log;
    access_log /var/log/nginx/lb_access.log;
}' >/etc/nginx/sites-available/round-robin


ln -s /etc/nginx/sites-available/round-robin /etc/nginx/sites-enabled/round-robin

service nginx restart
~~~
Lalu jalankan dan uji di client
## Soal 11
Lalu buat untuk setiap request yang mengandung /dune akan di proxy passing menuju halaman https://www.dunemovie.com.au/. (11)

## Soal 12
Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [Prefix IP].1.37, [Prefix IP].1.67, [Prefix IP].2.203, dan [Prefix IP].2.207. (12)

## Soal 13
Tidak mau kalah dalam perburuan spice, House atreides juga mengatur para pekerja di atreides.yyy.com.
Semua data yang diperlukan, diatur pada Chani dan harus dapat diakses oleh Leto, Duncan, dan Jessica. (13)
Menjalankan script berikut pada Chani
```
echo '# This group is read both by the client and the server
# use it for options that affect everything
[client-server]

# Import all .cnf files from configuration directory
!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mariadb.conf.d/

# Options affecting the MySQL server (mysqld)
[mysqld]
skip-networking=0
skip-bind-address
' > /etc/mysql/my.cnf

cd /etc/mysql/mariadb.conf.d/50-server.cnf

# Changes
bind-address            = 0.0.0.0 

service mysql restart

# Prompt for the MySQL root password
read -sp 'Enter MySQL root password: ' MYSQL_ROOT_PASSWORD
echo

# Execute MySQL commands
mysql -u root -p"$MYSQL_ROOT_PASSWORD" <<EOF
DROP USER IF EXISTS 'kelompokit20'@'%';
DROP USER IF EXISTS 'kelompokit20'@'localhost';
CREATE USER 'kelompokit20'@'%' IDENTIFIED BY 'passwordit20';
CREATE USER 'kelompokit20'@'localhost' IDENTIFIED BY 'passwordit20';
DROP DATABASE IF EXISTS dbkelompokit20;
CREATE DATABASE dbkelompokit20;
GRANT ALL PRIVILEGES ON dbkelompokit20.* TO 'kelompokit20'@'%';
GRANT ALL PRIVILEGES ON dbkelompokit20.* TO 'kelompokit20'@'localhost';
FLUSH PRIVILEGES;
EOF

echo "dbkelompokit20 created"
```
Kemudian mengecek apakah database telah bisa diakses pada laravel worker yaitu Leto, Duncan, dan Jessica dengan menggunakan command berikut
`mariadb --host=192.243.4.2 --port=3306 --user=kelompokit20 --password=passwordit20 dbkelompokit20`
Kemudian ketika MariaDB telah berjalan, menggunakan command `SHOW DATABASES;`

#### Dokumentasi pada Leto
<img width="539" alt="image" src="https://github.com/clar04/Jarkom-Modul-3-IT20-2024/assets/123356941/a7e731ff-2e9b-49a2-9732-9f7acafcb03a">


#### Dokumentasi pada Duncan
<img width="535" alt="image" src="https://github.com/clar04/Jarkom-Modul-3-IT20-2024/assets/123356941/7d1ea02c-c255-4544-9ded-9bc1b592ee72">


#### Dokumentasi pada Jessica
<img width="548" alt="image" src="https://github.com/clar04/Jarkom-Modul-3-IT20-2024/assets/123356941/31e87050-faa5-4b5a-bc33-f155975ad1eb">

## Soal 14
Leto, Duncan, dan Jessica memiliki atreides Channel sesuai dengan quest guide berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer (14)

## Soal 15
atreides Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada peta.
a. POST /auth/register (15)

## Soal 16
b. POST /auth/login (16)

## Soal 17
c. GET /me (17)

## Soal 18
Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur atreides Channel maka implementasikan Proxy Bind pada Stilgar untuk mengaitkan IP dari Leto, Duncan, dan Jessica. (18)

## Soal 19
Untuk meningkatkan performa dari Worker, coba implementasikan PHP-FPM pada Leto, Duncan, dan Jessica. Untuk testing kinerja naikkan 
- pm.max_children
- pm.start_servers
- pm.min_spare_servers
- pm.max_spare_servers
sebanyak tiga percobaan dan lakukan testing sebanyak 100 request dengan 10 request/second kemudian berikan hasil analisisnya pada PDF.(19)

## Soal 20
Nampaknya hanya menggunakan PHP-FPM tidak cukup untuk meningkatkan performa dari worker maka implementasikan Least-Conn pada Stilgar. Untuk testing kinerja dari worker tersebut dilakukan sebanyak 100 request dengan 10 request/second. (20)
