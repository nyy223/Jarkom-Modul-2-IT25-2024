# Jarkom-Modul-2-IT25-2024

# Anggota Kelompok
| Nama  | NRP  |
|----------|----------|
| Fikri Aulia As Sa'adi  | 5027231026 |
| Nayla Raissa Azzahra  | 5027231054 |

## Pembuatan Topologi
![Screenshot 2024-10-01 112926](https://github.com/user-attachments/assets/5a11cd09-9ea6-4940-947d-8b80714efaf3)

## Konfigurasi
```
--Nusantara--
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

--SRIWIJAYA--
auto eth0
iface eth0 inet static
	address 10.76.1.2
	netmask 255.255.255.0
	gateway 10.76.1.1

--HAYAMWURUK--
auto eth0
iface eth0 inet static
	address 10.76.1.3
	netmask 255.255.255.0
	gateway 10.76.1.1

--MAJAPAHIT--
auto eth0
iface eth0 inet static
	address 10.76.2.2
	netmask 255.255.255.0
	gateway 10.76.2.1

--SOLOK--
auto eth0
iface eth0 inet static
	address 10.76.2.3
	netmask 255.255.255.0
	gateway 10.76.2.1

--GajahMada--
auto eth0
iface eth0 inet static
	address 10.76.3.2
	netmask 255.255.255.0
	gateway 10.76.3.1

--ThomasAlfaEdison--
auto eth0
iface eth0 inet static
	address 10.76.3.3
	netmask 255.255.255.0
	gateway 10.76.3.1

--Tanjungkulai--
auto eth0
iface eth0 inet static
	address 10.76.1.4
	netmask 255.255.255.0
	gateway 10.76.1.1

--Bedahulu--
auto eth0
iface eth0 inet static
	address 10.76.1.5
	netmask 255.255.255.0
	gateway 10.76.1.1

--Kotalingga--
auto eth0
iface eth0 inet static
	address 10.76.1.6
	netmask 255.255.255.0
	gateway 10.76.1.1
```

## No. 1
Untuk mempersiapkan peperangan World War MMXXIV (Iya sebanyak itu), Sriwijaya membuat dua kotanya menjadi web server yaitu Tanjungkulai, dan Bedahulu, serta Sriwijaya sendiri akan menjadi DNS Master. Kemudian karena merasa terdesak, Majapahit memberikan bantuan dan menjadikan kerajaannya (Majapahit) menjadi DNS Slave. 
### Membuat Tanjungkulai dan Bedahulu menjadi webserver
#### Tanjungkulai dan Bedahulu
- Install apache2 melalui .bashrc
![Tanjungkulai](https://github.com/user-attachments/assets/7b086ac2-035c-442f-8167-a2aaf11aea5c)
- Coba curl melalui node lain untuk mengecek apakah apache berjalan
![Tanjungkulai(2)](https://github.com/user-attachments/assets/2c88144f-d2d0-4d89-8b87-443cfef7d07c)

### Membuat Sriwijaya dan Majapahit menjadi DNS Master dan Slave
- Install Bind9 melalui .bashrc
![image](https://github.com/user-attachments/assets/34ef6ca7-3b38-47f1-b70c-4cc4d9267a91)

## No. 2
### Membuat domain yang mengarah ke solok
- Membuat script untuk konfigurasi seperti berikut:
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
- Jalankan script
![image](https://github.com/user-attachments/assets/8544bcce-dee1-4422-b149-47a791f7505c)
- Tes di client
![image](https://github.com/user-attachments/assets/f3ca19fe-1ec2-40a0-8466-0f911b437693)

## No. 3
### Membuat domain yang mengarah ke Kotalingga
- Membuat script konfigurasi seperti berikut:
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
- Jalankan Script
![image](https://github.com/user-attachments/assets/2719aecd-2a6f-4604-8f09-fda9e466a5ba)
- Tes di client
![image](https://github.com/user-attachments/assets/d91a388c-6b2f-43db-917d-1b4d03b1463b)

## No. 4
### Membuat domain yang mengarah ke Tanjungkulai
- Membuat konfigurasi seperti berikut:
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
- Jalankan script
![image](https://github.com/user-attachments/assets/d217b26d-ad8a-4d8e-9c3e-f3ef5683d208)
- Tes di client
![image](https://github.com/user-attachments/assets/fafea9b7-14c9-4aca-8e8a-d5873870f2eb)

## No. 5
### Semua client sudah di tes di nomor 2,3, dan 4 dan berhasil melakukan ping

## No. 6
### Buat script untuk membuat reverse DNS mengakses domain pasopati.it25.com melalui alamat IP 10.76.1.6
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

### Buat script untuk ditaruh di semua client
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

### Test di client
```
host -t PTR 10.76.1.6
```

### Hasil di client
![Screenshot 2024-10-01 225451](https://github.com/user-attachments/assets/6468eb8b-ae55-4924-9698-7091e098c054)




