# Langkah - Langkah Instalasi dan Konfigurasi DHCP Server dengan DHCP Relay
DHCP-Relay pada Linux merupakan sebuah program managemen distribusi IP dari DHCP-Server. Pada Debian, DHCP-Relay merupakan sebuah metode untuk distribusi IP Address ke perangkat client dengan memanfaatkan DHCP server yang terpusat pada router lain. Sehingga bisa dikatakan router yang menjadi DHCP relay hanya meneruskan "DHCP Request" dari perangkat client ke DHCP server. Hal ini sangat membantu jika perangkat-perangkat client tidak berada dalam satu network dengan DHCP Server.

Contoh Topologi :
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/f35d631b-6734-4932-b5f4-ec6d8bc7f5fc)

## Prerequisite :
- Sudah terkonfigurasi IP Address di node server dan router

Konfigurasi IP Saya:

DHCP-Relay RTR (Node-1)
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/fedde992-cc4f-4f7a-87e9-4863f193155f)

DHCP-Server SRV (Node-2)
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/6c57443b-4460-48b3-8097-e2eed522c283)

## Instalasi DHCP-Server
Install `isc-dhcp-server` di Node yang menyediakan DHCP-Server. Untuk cara Installasi dan Konfigurasi DHCP-Server cek [disini](https://github.com/diotriandika/lnearher-public-repository/blob/edda35884e18f67bd06b6982805cebac8eb612e5/ASJ-Linux/DHCP-Server.md), akan tetapi terdapat perbedaan pada konfigurasi file `dhcpd.conf`

Edit file `dhcpd.conf`
```bash
sudo nano /etc/dhcp/dhcpd.conf
```
Ikuti format dibawah
```bash
uncomment :
 21 authoritative;

 30 subnet <ip-node-2> netmask <subnetmask-ip-node-2> {
 31 }

 52 # A slightly different configuration for an internal subnet.
 53 subnet <network-ip-dhcp> netmask <subnetmask-ip-dhcp> {
 54   range <start-ip-dhcp> <end-ip-dhcp>;
 55   option domain-name-servers <dns-nameserver>;
 56 #  option domain-name "internal.example.org";
 57   option routers <ip-gateway-dhcp>;
 58   option broadcast-address <ip-broadcast-dhcp>;
 59   default-lease-time 600;
 60   max-lease-time 7200;
 61 }
```
Contoh :
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/5580a79c-b84e-45d2-bd39-aa9bb5b49f63)
>
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/b971d4f2-58d8-4152-8fd7-6c5f5d40b9dc)

Exit dan save, kemudian restart service

## Installasi DHCP-Relay
Update package dengan `apt-get update`

Install `isc-dhcp-relay`
```bash
sudo apt-get install isc-dhcp-relay
```
Selanjutnya akan muncul menu seperti dibawah.
## Konfigurasi DHCP-Relay
Dsini masukan IP dari Node DHCP-Server. _Ingat, IP Dari Node/Host penyedia DHCP-Server, bukan IP yang akan didistribusikan_
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/42f7ec79-52f1-43d6-8901-6fd04e9d8543)

Pilih OK. 

Menu selanjutnya adalah memasukan informasi Interface mana yang akan dipakai untuk mendistribusikan DHCP-Server. Disini saya menggunakan `ens5` yang terhubung ke client
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/afa2f522-7f87-416b-b2a9-fb9a1c908a73)

Untuk menu selanjutnya bisa dikosongkan saja, dan pilih OK.

> Note : Jika salah dalam mengisi opsi - opsi diatas, kita bisa konfigurasi ulang dengan menjalankan command
> ```bash
> dpkg-reconfigure isc-dhcp-relay
> ```

Restart service `isc-dhcp-relay`
```bash
sudo systemctl restart isc-dhcp-relay
```
Terakhir kita aktifkan dengan menjalankan command 
```bash
sudo dhcrelays <ip-node-dhcp-server>
```
Contoh
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/c53240f2-afe2-493e-973d-40a3b5ca263a)

## Verifikasi
Cek client apakah sudah mendapatkan IP dari DHCP apa belum
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/0b38ce4e-b5ed-41c7-823b-9c371e35fb24)

**_Done_**
