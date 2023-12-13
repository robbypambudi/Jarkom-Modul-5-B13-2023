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
