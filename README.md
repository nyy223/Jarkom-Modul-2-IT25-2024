# Jarkom-Modul-2-IT25-2024

# Anggota Kelompok
| Nama  | NRP  |
|----------|----------|
| Fikri Aulia As Sa'adi  | 5027231026 |
| Nayla Raissa Azzahra  | 5027231054 |

## Pembuatan Topologi
<img width="907" alt="image" src="https://github.com/user-attachments/assets/17cc96f2-a066-4803-8184-61f0a1f6168b">

## Konfigurasi
#### Nusantara
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.76.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.76.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.76.3.1
	netmask 255.255.255.0
```
#### Sriwijaya
```
auto eth0
iface eth0 inet static
	address 10.76.1.2
	netmask 255.255.255.0
	gateway 10.76.1.1
```
#### HayamWuruk
```
auto eth0
iface eth0 inet static
	address 10.76.1.3
	netmask 255.255.255.0
	gateway 10.76.1.1
```
#### Majapahit
```
auto eth0
iface eth0 inet static
	address 10.76.2.2
	netmask 255.255.255.0
	gateway 10.76.2.1
```
#### Solok
```
auto eth0
iface eth0 inet static
	address 10.76.2.3
	netmask 255.255.255.0
	gateway 10.76.2.1
```
#### GajahMada
```
auto eth0
iface eth0 inet static
	address 10.76.3.2
	netmask 255.255.255.0
	gateway 10.76.3.1
```
#### ThomasAlfaEdison
```
auto eth0
iface eth0 inet static
	address 10.76.3.3
	netmask 255.255.255.0
	gateway 10.76.3.1
```
#### Tanjungkulai
```
auto eth0
iface eth0 inet static
	address 10.76.1.4
	netmask 255.255.255.0
	gateway 10.76.1.1
```
#### Bedahulu
```
auto eth0
iface eth0 inet static
	address 10.76.1.5
	netmask 255.255.255.0
	gateway 10.76.1.1
```
#### Kotalingga
```
auto eth0
iface eth0 inet static
	address 10.76.1.6
	netmask 255.255.255.0
	gateway 10.76.1.1
```

## No. 1
Untuk mempersiapkan peperangan World War MMXXIV (Iya sebanyak itu), Sriwijaya membuat dua kotanya menjadi web server yaitu Tanjungkulai, dan Bedahulu, serta Sriwijaya sendiri akan menjadi DNS Master. Kemudian karena merasa terdesak, Majapahit memberikan bantuan dan menjadikan kerajaannya (Majapahit) menjadi DNS Slave.

### Membuat Tanjungkulai dan Bedahulu menjadi webserver
Install apache2 melalui .bashrc di Tanjungkulai dan Bedahulu
![Tanjungkulai](https://github.com/user-attachments/assets/7b086ac2-035c-442f-8167-a2aaf11aea5c)

Coba curl melalui node lain untuk mengecek apakah apache berjalan
![Tanjungkulai(2)](https://github.com/user-attachments/assets/2c88144f-d2d0-4d89-8b87-443cfef7d07c)

### Setup .bashrc untuk mengubah Sriwijaya menjadi DNS Master dan Majapahit menjadi DNS Slave
#### Nusantara
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.76.0.0/16
```
#### Sriwijaya
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install bind9 -y
```
#### Majapahit
```
echo 'nameserver 192.168.122.1
nameserver 10.76.1.2' > /etc/resolv.conf // IP Master
apt-get update
apt-get install bind9 -y
```
#### HayamWuruk, GajahMada, ThomasAlfaEdison (Client)
```
echo -e '
nameserver 10.76.1.2 #IP Master
nameserver 10.76.2.2 #IP Slave
nameserver 192.168.122.1
' > /etc/resolv.conf
```
#### TanjungKulai, Bedahulu, Kotalingga, Solok
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
```
### Testing
Melakukan testing pada client dengan `ping google.com`
![WhatsApp Image 2024-10-02 at 19 11 45](https://github.com/user-attachments/assets/766b23aa-86e0-41dc-bbc7-7955c7ad320c)
![WhatsApp Image 2024-10-02 at 19 11 46](https://github.com/user-attachments/assets/a70b57ff-0985-4f14-acf4-1ec3702ad6e9)
![WhatsApp Image 2024-10-02 at 19 11 46 (1)](https://github.com/user-attachments/assets/7038add5-cd58-4195-b26e-760b0dd378fc)

## No. 2
Karena para pasukan membutuhkan koordinasi untuk melancarkan serangannya, maka buatlah sebuah domain yang mengarah ke Solok dengan alamat sudarsana.xxxx.com dengan alias www.sudarsana.xxxx.com, dimana xxxx merupakan kode kelompok. Contoh: sudarsana.it01.com.

#### Membuat domain yang mengarah ke solok dengan membuat script di Sriwijaya dengan konfigurasi seperti berikut:
>Sriwijaya/Jarkom2.sh
```
#!/bin/bash
apt update
apt install bind9 -y

echo 'zone "sudarsana.it25.com" {
	type master;
	file "/etc/bind/jarkom/sudarsana.it25.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/sudarsana.it25.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     sudarsana.it25.com. sudarsana.it25.com. (
                        2024100104      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      sudarsana.it25.com.
@       IN      A       10.76.2.3    ; IP Solok
www     IN      CNAME   sudarsana.it25.com.' > /etc/bind/jarkom/sudarsana.it25.com

service bind9 restart
```
#### Jalankan script
![image](https://github.com/user-attachments/assets/8544bcce-dee1-4422-b149-47a791f7505c)

#### Tes di client
![image](https://github.com/user-attachments/assets/f3ca19fe-1ec2-40a0-8466-0f911b437693)

## No. 3
Para pasukan juga perlu mengetahui mana titik yang akan diserang, sehingga dibutuhkan domain lain yaitu pasopati.xxxx.com dengan alias www.pasopati.xxxx.com yang mengarah ke Kotalingga.

#### Membuat domain yang mengarah ke Kotalingga dengan membuat script di Sriwijaya dengan konfigurasi seperti berikut:
>Sriwijaya/Jarkom3.sh
```
#!/bin/bash
apt update
apt install bind9 -y

echo 'zone "pasopati.it25.com" {
	type master;
	file "/etc/bind/jarkom/pasopati.it25.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/pasopati.it25.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     pasopati.it25.com. pasopati.it25.com. (
                        2024100104      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      pasopati.it25.com.
@       IN      A       10.76.1.6    ; IP Kotalingga
www     IN      CNAME   pasopati.it25.com.' > /etc/bind/jarkom/pasopati.it25.com

service bind9 restart
```
#### Jalankan Script
![image](https://github.com/user-attachments/assets/2719aecd-2a6f-4604-8f09-fda9e466a5ba)

#### Tes di client
![image](https://github.com/user-attachments/assets/d91a388c-6b2f-43db-917d-1b4d03b1463b)

## No. 4
Markas pusat meminta dibuatnya domain khusus untuk menaruh informasi persenjataan dan suplai yang tersebar. Informasi dan suplai meme terbaru tersebut mengarah ke Tanjungkulai dan domain yang ingin digunakan adalah rujapala.xxxx.com dengan alias www.rujapala.xxxx.com.

#### Membuat domain yang mengarah ke Tanjungkulai dengan membuat script di Sriwijaya dengan konfigurasi seperti berikut:
>Sriwijaya/Jarkom4.sh
```
#!/bin/bash
apt update
apt install bind9 -y

echo 'zone "rujapala.it25.com" {
	type master;
	file "/etc/bind/jarkom/rujapala.it25.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/rujapala.it25.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     rujapala.it25.com. rujapala.it25.com. (
                        2024100104      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      rujapala.it25.com.
@       IN      A       10.76.1.4    ; IP Tanjungkulai
www     IN      CNAME   rujapala.it25.com.' > /etc/bind/jarkom/rujapala.it25.com

service bind9 restart
```
#### Jalankan script
![image](https://github.com/user-attachments/assets/d217b26d-ad8a-4d8e-9c3e-f3ef5683d208)

#### Tes di client
![image](https://github.com/user-attachments/assets/fafea9b7-14c9-4aca-8e8a-d5873870f2eb)

## No. 5
Pastikan domain-domain tersebut dapat diakses oleh seluruh komputer (client) yang berada di Nusantara.

#### Melakukan testing dari client untuk semua domain
![WhatsApp Image 2024-10-03 at 04 00 24](https://github.com/user-attachments/assets/9a3be09d-da36-4592-87b9-111a3ad25fe6)
![WhatsApp Image 2024-10-03 at 04 00 25](https://github.com/user-attachments/assets/591b51c4-2534-4c19-9cfc-50349290733e)
![WhatsApp Image 2024-10-03 at 04 00 25 (1)](https://github.com/user-attachments/assets/544a5e5d-6086-47cb-9105-3387005d463b)

## No. 6
Beberapa daerah memiliki keterbatasan yang menyebabkan hanya dapat mengakses domain secara langsung melalui alamat IP domain tersebut. Karena daerah tersebut tidak diketahui secara spesifik, pastikan semua komputer (client) dapat mengakses domain pasopati.xxxx.com melalui alamat IP Kotalingga (Notes: menggunakan pointer record).

#### Buat script di Sriwijaya untuk membuat reverse DNS mengakses domain pasopati.it25.com melalui alamat IP 10.76.1.6
>Sriwijaya/Jarkom6.sh
```
#!/bin/bash

# Buat reverse DNS (Record PTR)
echo 'zone "1.76.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/1.76.10.in-addr.arpa";
};' >> /etc/bind/named.conf.local

cp /etc/bind/db.local /etc/bind/jarkom/1.76.10.in-addr.arpa

echo ';
; BIND data file for local loopback interface
;
$TTL 604800
@    IN    SOA    pasopati.it25.com. root.pasopati.it25.com. (
        2024050301    ; Serial
        604800        ; Refresh
        86400        ; Retry
        2419200      ; Expire
        604800 )     ; Negative Cache TTL
;
@    IN    NS    pasopati.it25.com.
6    IN    PTR   pasopati.it25.com.' > /etc/bind/jarkom/1.76.10.in-addr.arpa

service bind9 restart
```

#### Buat script untuk ditaruh di semua client
Hayamwuruk, Gajahmada, ThomasAlfaEdison/Jarkom6.sh
```
#!/bin/bash

# Set nameserver kembali ke IP Master
echo 'nameserver 192.168.122.1' > /etc/resolv.conf

# Install package dnsutils
apt-get update
apt-get install dnsutils -y

# Kembalikan nameserver ke IP Master & Slave
echo '
nameserver 10.76.1.2
nameserver 10.76.2.2' > /etc/resolv.conf
```

#### Test di client
```
host -t PTR 10.76.1.6
```
![Screenshot 2024-10-01 225451](https://github.com/user-attachments/assets/6468eb8b-ae55-4924-9698-7091e098c054)

## No. 7
Akhir-akhir ini seringkali terjadi serangan brainrot ke DNS Server Utama, sebagai tindakan antisipasi kamu diperintahkan untuk membuat DNS Slave di Majapahit untuk semua domain yang sudah dibuat sebelumnya yang mengarah ke Sriwijaya.

#### Membuat script di Sriwijaya sebagai DNS Master dengan also-notify dan allow-transfer agar memberikan izin kepada IP Slave
>Sriwijaya/Jarkom7.sh
```
#!/bin/bash

echo '
zone "sudarsana.it25.com" {
        type master;
        notify yes;
        also-notify { 10.76.2.2; }; //IP Majapahit
        allow-transfer { 10.76.2.2; }; //IP Majapahit
        file "/etc/bind/jarkom/sudarsana.it25.com";
};

zone "pasopati.it25.com" {
        type master;
        notify yes;
        also-notify { 10.76.2.2; }; //IP Majapahit
        allow-transfer { 10.76.2.2; }; //IP Majapahit
        file "/etc/bind/jarkom/pasopati.it25.com";
};

zone "rujapala.it25.com" {
        type master;
        notify yes;
        also-notify { 10.76.2.2; }; //IP Majapahit
        allow-transfer { 10.76.2.2; }; //IP Majapahit
        file "/etc/bind/jarkom/rujapala.it25.com";
};' > /etc/bind/named.conf.local

service bind9 restart
```
#### Membuat script di Majapahit sebagai DNS Slave
>Majapahit/Jarkom7.sh
```
#!/bin/bash

# Cek apakah bind9 sudah terinstal
if ! command -v named &> /dev/null
then
    echo "Bind9 belum terinstal, melakukan instalasi..."
    # Melakukan instalasi bind9
    apt-get update
    apt-get install bind9 -y
else
    echo "Bind9 sudah terinstal."
fi

echo '
zone "sudarsana.it25.com" {
    type slave;
    masters { 10.76.1.2; }; // IP Sriwijaya
    file "/var/lib/bind/sudarsana.it25.com";
};

zone "pasopati.it25.com" {
    type slave;
    masters { 10.76.1.2; }; // IP Sriwijaya
    file "/var/lib/bind/pasopati.it25.com";
};

zone "rujapala.it25.com" {
    type slave;
    masters { 10.76.1.2; }; // IP Sriwijaya
    file "/var/lib/bind/rujapala.it25.com";
};' > /etc/bind/named.conf.local

service bind9 restart
```
#### Mematikan bind9 pada Sriwijaya (master) untuk memastikan bahwa Majapahit (slave) dapat bekerja
`service bind9 stop`
![WhatsApp Image 2024-10-03 at 09 39 54](https://github.com/user-attachments/assets/f387fc10-fe02-4477-8158-2bb33b4c8261)

#### Melakukan testing dari client
![WhatsApp Image 2024-10-03 at 09 39 54 (1)](https://github.com/user-attachments/assets/44bbd360-868f-42ac-a6d2-cd655bc3ff78)

## No. 8
Kamu juga diperintahkan untuk membuat subdomain khusus melacak kekuatan tersembunyi di Ohio dengan subdomain cakra.sudarsana.xxxx.com yang mengarah ke Bedahulu.

#### Membuat script di Sriwijaya untuk menambah line untuk menyetting subdomain "cakra" di /etc/bind/jarkom/sudarsana.it25.com
![image](https://github.com/user-attachments/assets/a744dcbe-f021-4b0e-a5d1-20c593ec26b3)
>Sriwijaya/Jarkom8.sh
```
#!/bin/bash

# Tambahkan konfigurasi untuk membuat subdomain cakra.sudarsana.it25.com
echo '

; BIND data file for local loopback interface
$TTL 604800
@   IN  SOA sudarsana.it25.com. sudarsana.it25.com. (
            2024100104 ; Serial
            604800     ; Refresh
            86400      ; Retry
            2419200    ; Expire
            604800 )   ; Negative Cache TTL
;
@   IN  NS sudarsana.it25.com.
@   IN  A 10.76.2.3   ; IP Solok
www IN  CNAME sudarsana.it25.com.
cakra IN A 10.76.1.5  ; IP Bedahulu' > /etc/bind/jarkom/sudarsana.it25.com
```

#### Lakukan pengetesan subdomain di semua client
![Screenshot 2024-10-02 120532](https://github.com/user-attachments/assets/464f76e3-e053-41d8-9553-72f779ed0639)
![Screenshot 2024-10-02 120925](https://github.com/user-attachments/assets/1da1a8a0-f0cb-43af-9352-697e737616ef)
![Screenshot 2024-10-02 120943](https://github.com/user-attachments/assets/ffaff231-cc5c-4d34-8824-03f00a38247b)

## No. 9
Karena terjadi serangan DDOS oleh shikanoko nokonoko koshitantan (NUN), sehingga sistem komunikasinya terhalang. Untuk melindungi warga, kita diperlukan untuk membuat sistem peringatan dari siren man oleh Frekuensi Freak dan memasukkannya ke subdomain panah.pasopati.xxxx.com dalam folder panah dan pastikan dapat diakses secara mudah dengan menambahkan alias www.panah.pasopati.xxxx.com dan mendelegasikan subdomain tersebut ke Majapahit dengan alamat IP menuju radar di Kotalingga.

#### Membuat script di Sriwijaya untuk mengubah isi file /etc/bind/jarkom/pasopati.it25.com di Sriwijaya untuk menambah subdomain panah
Sriwijaya/Jarkom9.sh
```
#!/bin/bash

# Tambahkan konfigurasi untuk delegasi panah.pasopati.it25.com ke Majapahit
echo '
;
; BIND data file for local loopback interface
Sriwijaya
;
$TTL    604800
@       IN      SOA     pasopati.it25.com. pasopati.it25.com. (
                        2024050301      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      pasopati.it25.com.
@       IN      A       10.76.1.6     ; IP Kotalingga
www     IN      CNAME   pasopati.it25.com.
ns1     IN      A       10.76.2.2     ; Delegasikan ke Majapahit
panah   IN      NS      ns1' > /etc/bind/jarkom/pasopati.it25.com

echo '
options {
        directory "/var/cache/bind";

        //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

service bind9 restart
```

#### Membuat Script di Majapahit untuk membuat file baru untuk konfigurasi subdomain panah di pasopati.it25.com 
Majapahit/Jarkom9.sh
```
#!/bin/bash

# Tambahkan konfigurasi untuk delegasi panah.pasopati.it25.com ke Majapahit
echo '
options {
        directory "/var/cache/bind";

        //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

echo 'zone "panah.pasopati.it25.com" {
	type master;
	file "/etc/bind/panah/panah.pasopati.it25.com";
};' >> /etc/bind/named.conf.local

mkdir /etc/bind/panah

cp /etc/bind/db.local /etc/bind/panah/panah.pasopati.it25.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     panah.pasopati.it25.com. panah.pasopati.it25.com. (
                        2024050301      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      panah.pasopati.it25.com.
@       IN      A       10.76.1.6     ; IP Kotalingga
www     IN      CNAME   panah.pasopati.it25.com.' > /etc/bind/panah/panah.pasopati.it25.com

service bind9 restart
```

#### Lakukan testing pada client dengan ping panah.pasopati.1t25.com
![Screenshot 2024-10-02 153914](https://github.com/user-attachments/assets/ac76113d-c4f1-4684-932b-071312acec01)
![Screenshot 2024-10-02 154033](https://github.com/user-attachments/assets/b8ea72a0-6087-4d4c-bb9d-187d0c99ff79)
![image](https://github.com/user-attachments/assets/13df629e-7c15-43d1-95be-3828b8b95828)

## No. 10
Markas juga meminta catatan kapan saja meme brain rot akan dijatuhkan, maka buatlah subdomain baru di subdomain panah yaitu log.panah.pasopati.xxxx.com serta aliasnya www.log.panah.pasopati.xxxx.com yang juga mengarah ke Kotalingga.

#### Membuat script di Majapahit untuk memodifikasi file yang telah dibuat di nomor 9, yaitu /etc/bind/panah/panah.pasopati.it25.com di Majapahit. Script bertujuan untuk menambah domain log dan www.log
>Majapahit/Jarkom10.sh
```
#!/bin/bash

# Tambahkan konfigurasi untuk membuat subdomain log.panah.pasopati.it25.com
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     panah.pasopati.it25.com. panah.pasopati.it25.com. (
                        2024050301      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      panah.pasopati.it25.com.
@       IN      A       10.76.1.6     ; IP Kotalingga
www     IN      CNAME   panah.pasopati.it25.com.
log     IN      A       10.76.1.6     ; IP Kotalingga
www.log IN      CNAME   panah.pasopati.it25.com.' > /etc/bind/panah/panah.pasopati.it25.com

service bind9 restart
```

#### Lakukan testing ping log.panah.pasopati.it25.com dan www.log.panah.pasopati.it25.com di client
![Screenshot 2024-10-02 161018](https://github.com/user-attachments/assets/638bd445-e3a8-4617-9a24-338d0050e30a)
![Screenshot 2024-10-02 161554](https://github.com/user-attachments/assets/af6d3730-9b46-4a8b-b1c9-c5485b9b2792)

## No.11
Setelah pertempuran mereda, warga IT dapat kembali mengakses jaringan luar dan menikmati meme brainrot terbaru, tetapi hanya warga Majapahit saja yang dapat mengakses jaringan luar secara langsung. Buatlah konfigurasi agar warga IT yang berada diluar Majapahit dapat mengakses jaringan luar melalui DNS Server Majapahit.

#### Membuat script di Sriwijaya agar semuanya dapat mengakses jaringan luar melalui DNS Sriwijaya
>Sriwijaya/Jarkom11.sh
```
#!/bin/bash

# Tambahkan konfigurasi untuk DNS forwarder
echo '
options {
        directory "/var/cache/bind";

        forwarders {
                192.168.122.1; //IP Nusantara
        };
        //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

service bind9 restart
```

### Membuat script di Majapahit agar semuanya dapat mengakses jaringan luar melalui DNS Majapahit
Majapahit/Jarkom11.sh
```
#!/bin/bash

# Tambahkan konfigurasi untuk DNS forwarder
echo '
options {
        directory "/var/cache/bind";

        forwarders {
                192.168.122.1; //IP Nusantara
        };
        //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

service bind9 restart
```

#### Lakukan testing di semua client
![image](https://github.com/user-attachments/assets/0e2e2cb7-0cca-46ff-8298-b812518cc2ab)
![image](https://github.com/user-attachments/assets/464361ce-4995-46d7-bd03-bf4e92d43994)
![image](https://github.com/user-attachments/assets/4e3d977b-c0f9-481e-9da1-065babcabc7f)

## No.12
### Menambahkan IP Kotalingga di Sriwijaya dan Majapahit
> Sriwijaya, Majapahit / Jarkom12.sh
```
#!/bin/bash

# Tambahkan nameserver Ip Kotalingga
echo '
nameserver 192.168.122.1
nameserver 10.76.1.6' > /etc/resolv.conf
```
### Ini jelasin y
> Kotalingga/Jarkom12.sh
```
#!/bin/bash

# Tambahkan konfigurasi agar bisa deploy
# Cek apakah apache2 sudah terinstal
if ! command -v named &> /dev/null
then
    echo "Apache2 belum terinstal, melakukan instalasi..."
    # Melakukan instalasi apache2
    apt-get update
    apt-get install apache2 -y
    apt-get install libapache2-mod-php7.0 -y
else
    echo "apache2 sudah terinstal."
fi

# Cek apakah unzip sudah terinstal
if ! command -v named &> /dev/null
then
    echo "Unzip belum terinstal, melakukan instalasi..."
    apt-get update
    apt-get install unzip -y
else
    echo "unzip sudah terinstal."
fi

# Cek apakah php sudah terinstal
if ! command -v named &> /dev/null
then
    echo "PHP belum terinstal, melakukan instalasi..."
    # Melakukan instalasi php
    apt-get update
    apt-get install php -y
else
    echo "php sudah terinstal."
fi

# Download file lb.zip
curl -L -o lb.zip --insecure "https://drive.google.com/uc?export=download&id=1Sqf0TIiybYyUp5nyab4twy9svkgq8bi7"

# Hapus file template
rm -rf /var/www/html/index.php

# Copy file index.php
cp worker/index.php /var/www/html/index.php

# Restart Apache2 service
service apache2 restart
```
### Jelaskan ya nay
> Hayamwuruk, Gajahmada, ThomasAlfaEdisom/Jarkom12.sh
```
#!/bin/bash

# Tambahkan konfigurasi agar bisa membuka browser
if ! command -v named &> /dev/null
then
    echo "Lynx belum terinstal, melakukan instalasi..."
    apt-get update
    apt-get install lynx -y
else
    echo "lynx sudah terinstal."
fi
```
### Melakukan testing wlewlewle
![image](https://github.com/user-attachments/assets/ab587c97-644b-48c7-a5ee-cd0e57862d74)

![image](https://github.com/user-attachments/assets/049c151c-1174-4495-91c3-f7b6a8ef7bc9)


## No.13
### Menambahkan IP Solok di Sriwijaya dan Majapahit
> Sriwijaya, Majapahit/Jarkom13.sh
```
#!/bin/bash

# Tambahkan nameserver Ip Solok
echo '
nameserver 192.168.122.1
nameserver 10.76.2.3' > /etc/resolv.conf
```

### Jelasin y
> Bedahulu, Tanjungkulai/Jarkom13.sh
```
#!/bin/bash

# Tambahkan untuk keperluan load balancer
# Cek apakah apache2 sudah terinstal
if ! command -v named &> /dev/null
then
    echo "Apache2 belum terinstal, melakukan instalasi..."
    # Melakukan instalasi apache2
    apt-get update
    apt-get install apache2 -y
    apt-get install libapache2-mod-php7.0 -y
else
    echo "apache2 sudah terinstal."
fi

# Cek apakah unzip sudah terinstal
if ! command -v named &> /dev/null
then
    echo "Unzip belum terinstal, melakukan instalasi..."
    apt-get update
    apt-get install unzip -y
else
    echo "unzip sudah terinstal."
fi

# Cek apakah php sudah terinstal
if ! command -v named &> /dev/null
then
    echo "PHP belum terinstal, melakukan instalasi..."
    # Melakukan instalasi php
    apt-get update
    apt-get install php -y
else
    echo "php sudah terinstal."
fi

# Download file lb.zip
curl -L -o lb.zip --insecure "https://drive.google.com/uc?export=download&id=1Sqf0TIiybYyUp5nyab4twy9svkgq8bi7"

# Unzip file lb.zip
unzip lb.zip

# Hapus file template
rm -rf /var/www/html/index.php

# Copy file index.php
cp worker/index.php /var/www/html/index.php

# Restart Apache2 service
service apache2 restart
```

### Membuat script di Solok untuk.. untuk apa hayo
> Solok/Jarkom13.sh
```
#!/bin/bash

# Tambahkan keperluan untuk setting load balancer pada Mylta
# Cek apakah apache2 sudah terinstal
if ! command -v named &> /dev/null
then
    echo "Apache2 belum terinstal, melakukan instalasi..."
    # Melakukan instalasi apache2
    apt-get update
    apt-get install apache2 -y
    apt-get install libapache2-mod-php7.0 -y
else
    echo "apache2 sudah terinstal."
fi

# Cek apakah php sudah terinstal
if ! command -v named &> /dev/null
then
    echo "PHP belum terinstal, melakukan instalasi..."
    # Melakukan instalasi php
    apt-get update
    apt-get install php -y
else
    echo "php sudah terinstal."
fi

# Enable apache2 module
a2enmod proxy_balancer
a2enmod proxy_http
a2enmod lbmethod_byrequests

echo '
<VirtualHost *:80>
    <Proxy balancer://serverpool>
        BalancerMember http://10.76.1.4/
        BalancerMember http://10.76.1.5/
        BalancerMember http://10.76.1.6/
        Proxyset lbmethod=byrequests
    </Proxy>

    ProxyPass / balancer://serverpool/
    ProxyPassReverse / balancer://serverpool/
</VirtualHost>
' > /etc/apache2/sites-available/000-default.conf

service apache2 restart
```

### Testing loadbalancer kali ya tiap ngeping solok berubah ubah keren gak
![image](https://github.com/user-attachments/assets/3eac6066-a726-4fc4-8af0-4daa56a423d1)
![Screenshot 2024-10-02 225728](https://github.com/user-attachments/assets/d79f5cd2-1a20-4ea7-801b-cd3fbc0a4d73)
![image](https://github.com/user-attachments/assets/48163c2f-c9dd-4d47-ba34-a8a350497273)
![Screenshot 2024-10-02 225743](https://github.com/user-attachments/assets/4d569421-861e-4c52-a3f6-4e1e45766f77)
![image](https://github.com/user-attachments/assets/1bb7ace2-8f25-43e0-9b0a-a32669284ff0)
![Screenshot 2024-10-02 225757](https://github.com/user-attachments/assets/c5e15094-b8b9-498f-95ac-044afa68941c)

## No. 14
### Membuat script untuk Solok berisi:
- Mengentikan apache2
- Menginstall Nginx
- Memasukkan config ke nginx/sites-available
- Melakukan simlink dengan file konfigurasi dan menghapus file konfigurasi default
>Solok/Jarkom14.sh
```
service apache2 stop
apt-get update
apt-get install nginx -y

echo "upstream backend {
    server 10.76.1.4;
    server 10.76.1.5;
    server 10.76.1.6;
}

server {
    listen 80;
    server_name solok.it25.com www.solok.it25.com;

    location / {
        proxy_pass http://backend;
    }
}
" > /etc/nginx/sites-available/jarkom

ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled/jarkom

rm /etc/nginx/sites-enabled/default

service nginx restart
```

### Membuat script untuk Webserver berisi:
- Menginstall Nginx dan PHP
- Memasukkan konfigurasi yang sudah di download ke konfigurasi php (var/wwww/jarkom/index.php)
- Melakukan simlink untuk file konfigurasinya dan menghapus file konfigurasi default

>Tanjungkulai, Bedahulu, Kotalingga/Jarkom14.sh
```
service apache2 stop
apt-get update
apt-get install nginx -y
apt install php php-fpm php-mysql -y

mkdir /var/www/jarkom
cp /root/worker/index.php /var/www/jarkom/index.php

service php7.0-fpm start

echo -e "server {
    listen 80;

    root /var/www/jarkom;
    index index.php index.html index.htm index.nginx-debian.html;

    server_name _;

    location / {
        try_files \$uri \$uri/ /index.php?\$query_string;
    }

    location ~ \.php\$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}" > /etc/nginx/sites-available/jarkom

ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled/jarkom

rm /etc/nginx/sites-enabled/default

service nginx restart
```
![image](https://github.com/user-attachments/assets/fc69083c-fb79-47de-820b-0ae7ed3ed7c4)

## No.15
## No.16
### Membuat script untuk membuat konfigurasi baru solok.it25.com dengan isi mirip seperti No.2 karena sama-sama memberi domain ke Solok
>Sriwijaya / Jarkom16.sh
```
# Zona untuk domain solok.it25.com
echo -e 'zone "solok.it25.com" {
    type master;
    file "/etc/bind/jarkom/solok.it25.com";
};' >> /etc/bind/named.conf.local

# File zona untuk solok.it25.com
echo -e '
$TTL 604800
@       IN      SOA     solok.it25.com. root.solok.it25.com. (
                            2         ; Serial
                       604800         ; Refresh
                        86400         ; Retry
                      2419200         ; Expire
                       604800 )       ; Negative Cache TTL

@       IN      NS      solok.it25.com.
@       IN      A       10.76.2.3
www     IN      CNAME   solok.it25.com.
' > /etc/bind/jarkom/solok.it25.com

# Restart layanan BIND9
service bind9 restart
```
### Tes ping di semua client menggunakan domain www.solok.it25.com
![image](https://github.com/user-attachments/assets/6673e109-d660-43cc-b90f-2d6cfbc42e40)
![image](https://github.com/user-attachments/assets/99c71418-e981-483a-838e-08e5aea1ef85)
![image](https://github.com/user-attachments/assets/bb55d4f1-4ba5-40a9-87ff-0be454c8f910)

### Tes lynx menggunakan domain www.solok.it25.com
![image](https://github.com/user-attachments/assets/0a3406ab-45f0-415b-bb36-1e47f1be8af1)
![image](https://github.com/user-attachments/assets/c2e7de0d-e9cc-47a7-b1ee-c289e2c10b58)







