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

### Membuat Tanjungkulai dan Bedahulu menjadi webserver dengan install apache2 melalui .bashrc di Tanjungkulai dan Bedahulu
![Tanjungkulai](https://github.com/user-attachments/assets/7b086ac2-035c-442f-8167-a2aaf11aea5c)

### Coba curl melalui node lain untuk mengecek apakah apache berjalan
![Tanjungkulai(2)](https://github.com/user-attachments/assets/2c88144f-d2d0-4d89-8b87-443cfef7d07c)

### Install Bind9 melalui .bashrc di Sriwijaya dan Majapahit untuk menjadi DNS Master dan Slave
![image](https://github.com/user-attachments/assets/34ef6ca7-3b38-47f1-b70c-4cc4d9267a91)

## No. 2
### Membuat domain yang mengarah ke solok dengan membuat script untuk konfigurasi seperti berikut:
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
### Jalankan script
![image](https://github.com/user-attachments/assets/8544bcce-dee1-4422-b149-47a791f7505c)

### Tes di client
![image](https://github.com/user-attachments/assets/f3ca19fe-1ec2-40a0-8466-0f911b437693)

## No. 3
### Membuat domain yang mengarah ke Kotalingga dengan membuat script konfigurasi seperti berikut:
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
### Jalankan Script
![image](https://github.com/user-attachments/assets/2719aecd-2a6f-4604-8f09-fda9e466a5ba)

### Tes di client
![image](https://github.com/user-attachments/assets/d91a388c-6b2f-43db-917d-1b4d03b1463b)

## No. 4
### Membuat domain yang mengarah ke Tanjungkulai dengan membuat konfigurasi seperti berikut:
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
### Jalankan script
![image](https://github.com/user-attachments/assets/d217b26d-ad8a-4d8e-9c3e-f3ef5683d208)

### Tes di client
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

## No. 7
### Ayo kerjain nay

## No. 8
### Membuat script untuk menambah line untuk menyetting subdomain "cakra" di /etc/bind/jarkom/sudarsana.it25.com
![image](https://github.com/user-attachments/assets/a744dcbe-f021-4b0e-a5d1-20c593ec26b3)
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

### Lakukan pengetesan subdomain di semua client
![Screenshot 2024-10-02 120532](https://github.com/user-attachments/assets/464f76e3-e053-41d8-9553-72f779ed0639)
![Screenshot 2024-10-02 120925](https://github.com/user-attachments/assets/1da1a8a0-f0cb-43af-9352-697e737616ef)
![Screenshot 2024-10-02 120943](https://github.com/user-attachments/assets/ffaff231-cc5c-4d34-8824-03f00a38247b)

## No. 9
### Membuat script untuk mengubah isi file /etc/bind/jarkom/pasopati.it25.com di Sriwijaya untuk menambah subdomain panah
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

### Script untuk membuat file baru untuk konfigurasi subdomain panah di pasopati.it25.com 
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

### Lakukan pengetesan pada client dengan ping panah.pasopati.1t25.com
![Screenshot 2024-10-02 153914](https://github.com/user-attachments/assets/ac76113d-c4f1-4684-932b-071312acec01)
![Screenshot 2024-10-02 154033](https://github.com/user-attachments/assets/b8ea72a0-6087-4d4c-bb9d-187d0c99ff79)
![image](https://github.com/user-attachments/assets/13df629e-7c15-43d1-95be-3828b8b95828)

## No. 10
### Membuat script untuk memodifikasi file yang telah dibuat di nomor 9, yaitu /etc/bind/panah/panah.pasopati.it25.com di Majapahit. Script bertujuan untuk menambah domain log dan www.log
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

### Lakukan pengetesan ping log.panah.pasopati.it25.com dan www.log.panah.pasopati.it25.com di client
![Screenshot 2024-10-02 161018](https://github.com/user-attachments/assets/638bd445-e3a8-4617-9a24-338d0050e30a)
![Screenshot 2024-10-02 161554](https://github.com/user-attachments/assets/af6d3730-9b46-4a8b-b1c9-c5485b9b2792)

## No.11
### Membuat script agar semuanya dapat mengakses jaringan luar melalui DNS Sriwijaya

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

### Membuat script agar semuanya dapat mengakses jaringan luar melalui DNS Majapahit
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

### Lakukan testing di semua client
![image](https://github.com/user-attachments/assets/0e2e2cb7-0cca-46ff-8298-b812518cc2ab)
![image](https://github.com/user-attachments/assets/464361ce-4995-46d7-bd03-bf4e92d43994)
![image](https://github.com/user-attachments/assets/4e3d977b-c0f9-481e-9da1-065babcabc7f)






