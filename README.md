# Jarkom-Modul-2-IT25-2024

# Anggota Kelompok
| Nama  | NRP  |
|----------|----------|
| Fikri Aulia As Sa'adi  | 5027231026 |
| Nayla Raissa Azzahra  | 5027231054 |

## Pembuatan Topologi
![Screenshot 2024-10-01 112926](https://github.com/user-attachments/assets/5a11cd09-9ea6-4940-947d-8b80714efaf3)

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
### Membuat Tanjungkulai dan Bedahulu menjadi webserver
#### Tanjungkulai
- Menginstall apache2 dan melakukan start setiap kali node dinyalakan
![image](https://github.com/user-attachments/assets/bc08471b-b78a-44cf-a3c7-a2a6430c868e)
- Script disimpan di /root/.bashrc agar secara otomatis di run saat node dinyalakan
```
#!/bin/bash
# Script untuk konfigurasi Tanjungkulai sebagai Web Server

# Update repository dan install Apache2
apt update
apt install apache2 -y

# Aktifkan Apache agar berjalan otomatis saat boot
systemctl enable apache2

# Mulai layanan Apache
service apache2 start

# Verifikasi apakah Apache berjalan
service apache2 status

# Set IP Address dan Gateway
ip addr flush dev eth0
ip addr add 10.76.1.4/24 dev eth0
ip link set eth0 up
ip route add default via 10.76.1.1

# Verifikasi konfigurasi jaringan
ip addr show eth0
ip route
```

#### Bedahulu
- Langkah-langkah dan script kurang lebih sama seperti Tanjungkulai
```
#!/bin/bash
# Script untuk konfigurasi Bedahulu sebagai Web Server

# Update repository dan install Apache2
apt update
apt install apache2 -y

# Aktifkan Apache agar berjalan otomatis saat boot
systemctl enable apache2

# Mulai layanan Apache
service apache2 start

# Verifikasi apakah Apache berjalan
service apache2 status

# Set IP Address dan Gateway
ip addr flush dev eth0
ip addr add 10.76.1.5/24 dev eth0
ip link set eth0 up
ip route add default via 10.76.1.1

# Verifikasi konfigurasi jaringan
ip addr show eth0
ip route
```

### Mengubah Sriwijaya Menjadi DNS Master
- Buat script untuk menginstall Bind9 serta melakukan konfigurasi file agar menjadi DNS Master
![image](https://github.com/user-attachments/assets/e27cbdae-39d8-4be0-b27d-b6bbb704fd6b)

```
#!/bin/bash
# Script untuk konfigurasi DNS Master di Sriwijaya

# Update repository dan install BIND9
apt update
apt install bind9 -y

# Aktifkan BIND9 agar berjalan otomatis saat boot
systemctl enable bind9

# Konfigurasi DNS Master untuk it25.com
cat <<EOL > /etc/bind/named.conf.local
zone "it25.com" {
    type master;
    file "/etc/bind/db.it25.com";
};
EOL

# Buat file zona untuk it25.com
cat <<EOL > /etc/bind/db.it25.com
;
; BIND data file for it25.com
;
\$TTL    604800
@       IN      SOA     ns.it25.com. admin.it25.com. (
                           2023100101         ; Serial
                                604800         ; Refresh
                                 86400         ; Retry
                               2419200         ; Expire
                                604800 )       ; Negative Cache TTL
;
@       IN      NS      ns.it25.com.
@       IN      A       10.76.1.2  ; IP Sriwijaya sebagai DNS Master
ns      IN      A       10.76.1.2  ; Sriwijaya sebagai DNS Server

tanjungkulai     IN      A       10.76.1.4
bedahulu         IN      A       10.76.1.5
kotalingga       IN      A       10.76.1.6
EOL

# Restart BIND untuk menerapkan perubahan
service bind9 restart

# Verifikasi apakah BIND berjalan
service bind9 status

# Set IP Address dan Gateway
ip addr flush dev eth0
ip addr add 10.76.1.2/24 dev eth0
ip link set eth0 up
ip route add default via 10.76.1.1
```

### Mengubah Majapahit Menjadi DNS Slave
- Buatlah script dengan konfigurasi hampir sama seperti Sriwijaya dengan sedikit perubahan
![image](https://github.com/user-attachments/assets/53f839a7-7e0b-489e-9e46-cb220763dea1)
```
#!/bin/bash
# Script untuk konfigurasi DNS Slave di Majapahit

# Update repository dan install BIND9
apt update
apt install bind9 -y

# Aktifkan BIND9 agar berjalan otomatis saat boot
systemctl enable bind9

# Konfigurasi DNS Slave untuk it25.com
cat <<EOL > /etc/bind/named.conf.local
zone "it25.com" {
    type slave;
    file "/var/cache/bind/db.it25.com";
    masters { 10.76.1.2; };  # IP DNS Master di Sriwijaya
};
EOL

# Restart BIND untuk menerapkan perubahan
service bind9 restart

# Verifikasi apakah BIND berjalan
service bind9 status

# Set IP Address dan Gateway
ip addr flush dev eth0
ip addr add 10.76.2.2/24 dev eth0
ip link set eth0 up
ip route add default via 10.76.2.1
```



