# Jarkom-Modul-5-B13-2023

- Robby Ulung Pambudi
- Tsaqif Daniar

## Table of Contents

- [VLSM](#vlsm)

## Pembagian Subnet

![Pembagian Subnet](/assets/pembagian-ip.png)
Keterangan :

- Richter adalah DNS Server
- Revolte adalah DHCP Server
- Sein dan Stark adalah Web Server
- Jumlah Host pada SchwerMountain adalah 64
- Jumlah Host pada LaubHills adalah 255
- Jumlah Host pada TurkRegion adalah 1022
- Jumlah Host pada GrobeForest adalah 512

## VLSM (Variable Length Subnet Masking)

Menentukan jumlah alamat IP yang dibutuhkan oleh tiap subnet dan melabel netmask berdasarkan jumlah IP yang dibutuhkan.

| **Subnet ** | **Jumlah IP** | **Netmask** |
| :---------: | :-----------: | :---------: |
|     A1      |       2       |     /30     |
|     A2      |       2       |     /30     |
|     A3      |      66       |     /25     |
|     A4      |      256      |     /24     |
|     A5      |       2       |     /30     |
|     A6      |       2       |     /30     |
|     A7      |       2       |     /30     |
|     A8      |       2       |     /30     |
|     A9      |     1023      |     /21     |
|     A10     |      514      |     /22     |
|  **Total**  |   **1871**    |   **/20**   |

## Pohon IP

Subnet besar yang dibentuk memiliki NID 10.15.0.0 dengan netmask /20 sehingga pembagian IP berdasarkan NID dan netmask dihitung sesuai dengan pohon berikut.
![Pembagian Subnet](/assets/tree.png)

Pada VLSM ini diturunkan sesuai dengan length atasnya sehingga ketika /20 akan diturunkan menjadi /21, dan pembagian IPnya mengikuti tabel. Kemudian jika ada subnet yang bisa diassign, maka kita langsung mengassign. Hal ini dilakukan berulang" sampai semua subnet selesai di assign.

## Subnet + IP

Selanjutnya menyesuaikan pembagian IP sesuai dengan subnet yang telah dibagi berdasarkan pohon diatas pada topologi. Setelah itu akan didapatkan pembagian IP untuk tiap nodenya.

| Subnet | Node           | IP          | Netmask         | Length | NID         | Broadcast    |
| ------ | -------------- | ----------- | --------------- | ------ | ----------- | ------------ |
| A1     | Fern           | 10.15.0.1   | 255.255.255.252 | 30     | 10.15.0.0   | 10.15.0.3    |
|        | Revolte        | 10.15.0.2   | 255.255.255.252 | 30     |             |              |
|        |                |             |                 |        |             |              |
| A2     | Fern           | 10.15.0.5   | 255.255.255.252 | 30     | 10.15.0.4   | 10.15.0.7    |
|        | Richter        | 10.15.0.6   | 255.255.255.252 | 30     |             |              |
|        |                |             |                 |        |             |              |
| A3     | Himmel         | 10.15.0.129 | 255.255.255.128 | 25     | 10.15.0.128 | 10.15.0.255  |
|        | Fern           | 10.15.0.130 | 255.255.255.128 | 25     |             |              |
|        | SchwerMountain | DHCP        | 255.255.255.128 | 25     |             |              |
|        |                |             |                 |        |             |              |
| A4     | Himmel         | 10.15.1.1   | 255.255.255.000 | 24     | 10.15.1.0   | 10.15.1.255  |
|        | LaubHills      | DHCP        | 255.255.255.000 | 24     |             |              |
|        |                |             |                 |        |             |              |
| A5     | Frieren        | 10.15.0.9   | 255.255.255.252 | 30     | 10.15.0.8   | 10.15.0.11   |
|        | Himmel         | 10.15.0.10  | 255.255.255.252 | 30     |             |              |
|        |                |             |                 |        |             |              |
| A6     | Frieren        | 10.15.0.13  | 255.255.255.252 | 30     | 10.15.0.12  | 10.15.0.15   |
|        | Stark          | 10.15.0.14  | 255.255.255.252 | 30     |             |              |
|        |                |             |                 |        |             |              |
| A7     | Aura           | 10.15.0.17  | 255.255.255.252 | 30     | 10.15.0.16  | 10.15.0.19   |
|        | Frieren        | 10.15.0.18  | 255.255.255.252 | 30     |             |              |
|        |                |             |                 |        |             |              |
| A8     | Aura           | 10.15.0.21  | 255.255.255.252 | 30     | 10.15.0.20  | 10.15.0.23   |
|        | Heiter         | 10.15.0.22  | 255.255.255.252 | 30     |             |              |
|        |                |             |                 |        |             |              |
| A9     | Heiter         | 10.15.8.1   | 255.255.248.000 | 21     | 10.15.8.0   | 10.15.15.255 |
|        | TurkRegion     | DHCP        | 255.255.248.000 | 21     |             |              |
|        |                |             |                 |        |             |              |
| A10    | Heiter         | 10.15.4.1   | 255.255.252.000 | 22     | 10.15.4.0   | 10.15.7.255  |
|        | Sein           | 10.15.4.2   | 255.255.252.000 | 22     |             |              |
|        | GrabForest     | DHCP        | 255.255.252.000 | 22     |             |              |

## Topologi Pada GNS

![Topologi](/assets/gns.png)

## Konfigurasi Router + IP

### Konfigurasi ETH

- Revolte

```bash
# Static config for eth0
auto eth0
iface eth0 inet static
	address 10.15.0.2
	netmask 255.255.255.252
	gateway 10.15.0.1
	up echo nameserver 10.15.0.6 > /etc/resolv.conf
```

- Richter

```bash
# Static config for eth0
auto eth0
iface eth0 inet static
	address 10.15.0.6
	netmask 255.255.255.252
	gateway 10.15.0.5
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- Fern

```bash
# Static config for eth0
auto eth0
iface eth0 inet static
	address 10.15.0.130
	netmask 255.255.255.128
	gateway 10.15.0.129
	up echo nameserver 10.15.0.6 > /etc/resolv.conf


# Static config for eth1
auto eth1
iface eth1 inet static
	address 10.15.0.5
	netmask 255.255.255.252


# Static config for eth2
auto eth2
iface eth2 inet static
	address 10.15.0.1
	netmask 255.255.255.252
```

- SchewerMountain1

```bash
# DHCP config for eth0
auto eth0
iface eth0 inet dhcp
```

- LaubHills

```bash
# DHCP config for eth0
auto eth0
iface eth0 inet dhcp
```

- Himmel

```bash
# Static config for eth0
auto eth0
iface eth0 inet static
         address 10.15.0.10
         netmask 255.255.255.252
         gateway 10.15.0.9
	 up echo nameserver 10.15.0.6 > /etc/resolv.conf

auto eth1
iface eth1 inet static
         address 10.15.0.129
         netmask 255.255.255.128

auto eth2
iface eth2 inet static
         address 10.15.1.1
         netmask 255.255.255.0
```

- Frieren

```bash
# Static config for eth0
auto eth0
iface eth0 inet static
         address 10.15.0.18
         netmask 255.255.255.252
         gateway 10.15.0.17
	 up echo nameserver 10.15.0.6 > /etc/resolv.conf

auto eth1
iface eth1 inet static
         address 10.15.0.13
         netmask 255.255.255.252

auto eth2
iface eth2 inet static
         address 10.15.0.9
         netmask 255.255.255.252
```

- Stark

```
# Static config for eth0
auto eth0
iface eth0 inet static
         address 10.15.0.14
         netmask 255.255.255.252
         gateway 10.15.0.13
	 up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- Aura

```bash
auto eth0
iface eth0 inet static
    address 192.168.122.14
    netmask 255.255.255.0
    gateway 192.168.122.1

auto eth2
iface eth2 inet static
       address 10.15.0.17
       netmask 255.255.255.252

auto eth1
iface eth1 inet static
      address 10.15.0.21
      netmask 255.255.255.252
```

- Heiter

```bash
auto eth0
iface eth0 inet static
         address 10.15.0.22
         netmask 255.255.255.252
         gateway 10.15.0.21
	 up echo nameserver 192.168.122.1 > /etc/resolv.conf

auto eth2
iface eth2 inet static
         address 10.15.8.1
         netmask 255.255.248.0

auto eth1
iface eth1 inet static
         address 10.15.4.1
         netmask 255.255.252.0
```

- TurkRegion

```bash
# DHCP config for eth0
auto eth0
iface eth0 inet dhcp
```

- Sein

```bash
# Static config for eth0
auto eth0
iface eth0 inet static
	address 10.15.4.2
	netmask 255.255.252.0
	gateway 10.15.4.1
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- GrabForest

```bash
# DHCP config for eth0
auto eth0
iface eth0 inet dhcp
```

## Routing

- Himmel

```
route add -net 10.15.0.0 netmask 255.255.255.252 gw 10.15.0.130
route add -net 10.15.0.4 netmask 255.255.255.252 gw 10.15.0.130
```

- Frieren

```
route add -net 10.15.0.0 netmask 255.255.255.252 gw 10.15.0.10
route add -net 10.15.0.4 netmask 255.255.255.252 gw 10.15.0.10
route add -net 10.15.0.128 netmask 255.255.255.128 gw 10.15.0.10
route add -net 10.15.1.0 netmask 255.255.255.0 gw 10.15.0.10
```

- Aura

```
route add -net 10.15.0.0 netmask 255.255.255.252 gw 10.15.0.18
route add -net 10.15.0.4 netmask 255.255.255.252 gw 10.15.0.18
route add -net 10.15.0.128 netmask 255.255.255.128 gw 10.15.0.18
route add -net 10.15.1.0 netmask 255.255.255.0 gw 10.15.0.18
route add -net 10.15.0.8 netmask 255.255.255.252 gw 10.15.0.18
route add -net 10.15.0.12 netmask 255.255.255.252 gw 10.15.0.18

route add -net 10.15.8.0 netmask 255.255.248.0 gw 10.15.0.22
route add -net 10.15.4.0 netmask 255.255.252.0 gw 10.15.0.22
```

> Yang lainya tidak perlu karena sudah terhubung dengan router yang lain. Contoh : Sein sudah terhubung dengan Heiter, sehingga tidak perlu di routing lagi. Begitu juga dengan GrabForest dan TurkRegion.

## Konfigurasi DHCP

### DHCP Server

Pada DHCP Server, kita akan menginstall isc-dhcp-server dengan perintah

```bash
apt-get update
wait
apt-get install isc-dhcp-server -y
wait

echo '
INTERFACESv4="eth0"
INTERFACESv6=""
' > /etc/default/isc-dhcp-server

cp ~/dhcpd.conf /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```

Kemudian pada file dhcpd.conf, kita akan mengkonfigurasi seperti berikut

```bash

option domain-name "example.org";
option domain-name-servers ns1.example.org, ns2.example.org;

default-lease-time 600;
max-lease-time 7200;

ddns-update-style none;

subnet 10.15.0.0 netmask 255.255.255.252 {
}

subnet 10.15.0.128 netmask 255.255.255.128 {
    range 10.15.0.131 10.15.0.254;
    option routers 10.15.0.129;
    option broadcast-address 10.15.0.255;
    option domain-name-servers 10.15.0.6;
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 10.15.1.0 netmask 255.255.255.0 {
    range 10.15.1.2 10.15.1.254;
    option routers 10.15.1.1;
    option broadcast-address 10.15.1.255;
    option domain-name-servers 10.15.0.6;
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 10.15.8.0 netmask 255.255.248.0 {
    range 10.15.8.2 10.15.15.254;
    option routers 10.15.8.1;
    option broadcast-address 10.15.15.255;
    option domain-name-servers 10.15.0.6;
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 10.15.4.0 netmask 255.255.252.0 {
    range 10.15.4.3 10.15.7.254;
    option routers 10.15.4.1;
    option broadcast-address 10.15.7.255;
    option domain-name-servers 10.15.0.6;
    default-lease-time 180;
    max-lease-time 5760;
}
```

- `option domain-name-servers` adalah DNS Server yang akan digunakan
- `default-lease-time` adalah waktu lease default
- `max-lease-time` adalah waktu lease maksimal
- `subnet` adalah subnet yang akan digunakan
- `range` adalah range IP yang akan digunakan
- `option routers` adalah IP dari router yang akan digunakan
- `option broadcast-address` adalah IP broadcast yang akan digunakan

### DHCP Relay

Install isc-dhcp-relay pada route **Himmel dan Heiter** dengan perintah

```bash
# Configuration DHCP Relay
apt-get update
wait
apt-get install isc-dhcp-relay -y
wait

echo '
SERVERS="10.15.0.2"
INTERFACES="eth0 eth1 eth2"
OPTIONS="-m replace"
' >  /etc/default/isc-dhcp-relay

echo '
net.ipv4.ip_forward=1
' >  /etc/sysctl.conf
```

- `SERVERS` adalah IP dari DHCP Server yang akan digunakan
- `INTERFACES` adalah interface yang akan digunakan untuk DHCP Relay
- `OPTIONS` adalah opsi yang digunakan untuk DHCP Relay
- `net.ipv4.ip_forward=1` adalah opsi untuk mengaktifkan ip forwarding

## Konfigurasi DNS

```
apt-get update
wait

apt-get install bind9 -y
wait
cp ~/named.conf.options /etc/bind/

service bind9 restart
```

- `named.conf.options` adalah konfigurasi DNS Server

Berikut adalah isi dari file `named.conf.options`

```
options {
        directory "/var/cache/bind";

        forwarders {
                192.168.122.1;
        };

        allow-query{any;};
        auth-nxdomain no;
        listen-on-v6 { any; };
};
```

- `forwarders` adalah DNS Server yang akan digunakan untuk forward
- `allow-query` adalah opsi untuk mengizinkan query dari semua IP
- `auth-nxdomain no` adalah opsi untuk mengizinkan domain yang tidak ada

## Konfigurasi Web Server

Untuk konfigurasi web server, kita akan menginstall nginx pada node **Sein dan Stark** dengan perintah

```bash
apt-get update
wait
apt-get install nginx -y
wait
cp ~/index.nginx-debian.html /var/www/html
service nginx start
apt-get install netcat -y
```

- `index.nginx-debian.html` adalah file html yang akan ditampilkan pada web server nanti
- `netcat` adalah aplikasi yang digunakan untuk mengirimkan data ke web server

## Soal

### Pemasalah 1

Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Aura menggunakan iptables, tetapi tidak ingin menggunakan MASQUERADE.

### Solusi

Pada node **Aura**, kita akan mengkonfigurasi iptables dengan perintah

```bash
iptables -t nat -A POSTROUTING -s 10.15.0.0/20 -o eth0 -j SNAT --to-source 192.168.122.14
```

- `-t nat` adalah opsi untuk menambahkan rule pada tabel nat
- `-A POSTROUTING` adalah opsi untuk menambahkan rule pada chain POSTROUTING
- `-s` adalah opsi untuk menentukan source IP
- `-o` adalah opsi untuk menentukan interface
- `-j SNAT` adalah opsi untuk melakukan SNAT
- `--to-source` adalah opsi untuk menentukan IP yang akan digunakan untuk SNAT

### Testing

Pada node **Sein**, kita akan melakukan ping ke google.com dengan perintah

```bash
ping google.com
```

https://github.com/robbypambudi/Jarkom-Modul-5-B13-2023/assets/34505233/56df455d-cb96-45df-929b-6d763b15fb1b

### Pemasalah 2

Kalian diminta untuk melakukan drop semua TCP dan UDP kecuali port 8080 pada TCP.

### Solusi

Untuk melakukan drop semua TCP dan UDP kecuali port 8080 pada TCP, kita akan mengkonfigurasi iptables pada node **SchewerMountain** dengan perintah

```bash
# Berisikhkan semua aturan yang ada jika diperlukan
iptables -F
# Izinkan koneksi yang masuk pada port 8080 TCP
iptables -A INPUT -p tcp --dport 8080 -j ACCEPT

# Jatuhkan semua koneksi TCP yang tidak menuju ke port 8080
iptables -A INPUT -p tcp ! --dport 8080 -j DROP

# Jatuhkan semua koneksi UDP
iptables -A INPUT -p udp -j DROP
```

- `iptables -F` adalah opsi untuk menghapus semua rule yang ada

- `iptables -A INPUT -p tcp --dport 8080 -j ACCEPT` adalah opsi untuk menambahkan rule pada chain INPUT untuk menerima koneksi TCP pada port 8080

- `iptables -A INPUT -p tcp ! --dport 8080 -j DROP` adalah opsi untuk menambahkan rule pada chain INPUT untuk menolak koneksi TCP yang tidak menuju ke port 8080

### Testing

Untuk melakukan testing, kita akan menggunakan node **SchewerMountain** dan **Revolte**. Untuk membuat perbandingan antara port yang belum di drop dan port yang sudah di drop, kita akan menggunakan netcat pada node **Revolte** dengan perintah

Perhatikan video berikut untuk melihat perbandingan antara port yang belum di drop dan port yang sudah di drop

https://github.com/robbypambudi/Jarkom-Modul-5-B13-2023/assets/34505233/25ea1a47-510a-4dec-bce0-f743b125f805

### Pemasalah 3

Kepala Suku North Area meminta kalian untuk membatasi DHCP dan DNS Server hanya dapat dilakukan ping oleh maksimal 3 device secara bersamaan, selebihnya akan di drop.

### Solusi

Untuk membatasi DHCP dan DNS Server hanya dapat dilakukan ping oleh maksimal 3 device secara bersamaan, kita akan mengkonfigurasi iptables pada node **Revolte** dan **Richter** dengan perintah

```bash
# Soal No 3
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
```

- `iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP` adalah opsi untuk menambahkan rule pada chain INPUT untuk menolak koneksi ICMP yang lebih dari 3
- `--connlimit-above 3` adalah opsi untuk menentukan batas koneksi
- `--connlimit-mask 0` adalah opsi untuk menentukan mask

### Testing

Untuk melakukan testing, kita akan menggunakan node **Revolte** dan **Richter**. Untuk membuat perbandingan antara koneksi yang belum di batasi dan koneksi yang sudah di batasi, kita akan menggunakan netcat pada node **Revolte** dan **Richter** dengan perintah

- Testing pada node **Revolte**

`ping ping 10.15.0.2` dan mendapatkan hasil sebagai berikut :

https://github.com/robbypambudi/Jarkom-Modul-5-B13-2023/assets/34505233/77e97ae5-3f1f-49e7-a45b-bce04083ca75

    - Dari video diatas, kita dapat melihat bahwa koneksi ICMP yang lebih dari 3 akan di drop

- Testing pada node **Richter**

`ping 10.15.0.2` dan mendapatkan hasil sebagai berikut :

https://github.com/robbypambudi/Jarkom-Modul-5-B13-2023/assets/34505233/d058e54a-a097-4e40-85bc-81d66602dfdb

    - Dari video diatas, kita dapat melihat bahwa koneksi ICMP yang lebih dari 3 akan di drop
