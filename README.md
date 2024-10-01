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

### Membuat Tanjungkulai dan Bedahulu menjadi web server
- Install apache di kedua node
![no 1 tanjungkulai (2)](https://github.com/user-attachments/assets/a4a26759-77c3-42ea-8d57-01a928c43eda)
![no 1 bedahulu (2)](https://github.com/user-attachments/assets/0d4f0b92-0646-46c3-a868-7bef68f49cc5)

- Menguji dengan curl di node lain
![Screenshot 2024-10-01 124908](https://github.com/user-attachments/assets/faf3eeed-30f3-4d20-8e75-0aa7030941e1)

### Membuat Sriwijaya menjadi DNS Master
- Install Bind9
![no 1 sriwijaya](https://github.com/user-attachments/assets/9ee41855-436c-4fa8-941f-a183c0185492)
- Mengubah konfigurasi /etc/bind/named.conf.local
![no 1 sriwijaya (2)](https://github.com/user-attachments/assets/49fd93a6-7662-4dbb-b2c6-6be675626c88)
- Mengubah konfigurasi /etc/bind/db.it25.com
![no 1 sriwijaya](https://github.com/user-attachments/assets/b7f0bcf8-9f09-4dff-8c75-1c052dcaf5b1)
- Tes di node lain apakah berhasil, disini kami mencoba di node Bedahulu
![no 1 sriwijaya (4)](https://github.com/user-attachments/assets/3a0b51b4-3aaa-4231-8d7d-bad347e4f699)




