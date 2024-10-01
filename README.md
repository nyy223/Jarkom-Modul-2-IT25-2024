![image](https://github.com/user-attachments/assets/51a76ef5-8f20-433b-89fd-e3ff3a2e9b66)![image](https://github.com/user-attachments/assets/228fa198-64c4-46b6-bb64-4c984a2aa01d)![image](https://github.com/user-attachments/assets/e4379db1-bfe2-4506-89b7-8794d85fabd7)# Jarkom-Modul-2-IT25-2024

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
tulis sini scriptnya terus jelasin y
```
- Jalankan script
![image](https://github.com/user-attachments/assets/8544bcce-dee1-4422-b149-47a791f7505c)
- Tes di client
![image](https://github.com/user-attachments/assets/f3ca19fe-1ec2-40a0-8466-0f911b437693)

## No. 3
### Membuat domain yang mengarah ke Kotanigga
- Membuat script konfigurasi seperti berikut:
```
awoakwoka
```
- Jalankan Script
![image](https://github.com/user-attachments/assets/2719aecd-2a6f-4604-8f09-fda9e466a5ba)
- Tes di client
![image](https://github.com/user-attachments/assets/d91a388c-6b2f-43db-917d-1b4d03b1463b)

## No. 4
### Membuat domain yang mengarah ke Tanjungkulai
- Membuat konfigurasi seperti berikut:
```
jelasin yy
```
- Jalankan script
![image](https://github.com/user-attachments/assets/d217b26d-ad8a-4d8e-9c3e-f3ef5683d208)
- Tes di client
![image](https://github.com/user-attachments/assets/fafea9b7-14c9-4aca-8e8a-d5873870f2eb)

## No. 5
### Semua client sudah di tes di nomor 2,3, dan 4 dan berhasil melakukan ping





