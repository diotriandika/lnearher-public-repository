# Langkah - Langkah Instalasi dan Konfigurasi Email Server dengan Postfix SMTP
Postfix sebagai SMTP-Server adalah sebuah MTA (Message Transfer Agent) yang menyediakan layanan untuk mengirimkian email.

- Pastikan IP Address sudah terkonfigurasi
- FQDN sudah diset
  
> Note : Sebelum instalasi Roundcube, saya rekomendasikan untuk menyiapkan alamat domain yang nantinya digunakan untuk konfigurasi. Contohnya saya disini menggunakan domain `mail.itnsa.id` yang saya konfigurasi lokal dengan bind 9
>
> Cek [disini](https://github.com/diotriandika/learn-networking/blob/6a6cfd1a5342b826d3e62469d0584ba5e9f5d1d5/Basic%20Configuration%20Linux/Assesmen%20Sistem%20Jaringan%20XI/DNS-Server.md) untuk cara set-up domain dengan bind9

## Instalasi Postfix
Update list package dengan `apt-get update` kemudian install postfix
```
sudo apt install postfix -y
```
Akan muncul menu seperti dibawah, select OK dan menu selanjutnya pilih Internet Site
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/3e390e38-7831-4ac2-b062-cd7d626cf8a4)
>
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/a0e21bdb-8a31-43db-ac4d-b29483c51fce)

Selanjutnya masukan domain name di menu dibawah, ini nantinya akan menjadi domain pada alamat e-mail nantinya. Contohnya disini saya menggunakan `itnsa.id` yang dimana nantinya akan menjadi alamat email `user@itnsa.id`.
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/723f6721-afd6-4197-85c5-2c11b47ab581)

## Konfigurasi Postfix
Edit file `main.cf` untuk mengalokasikan direktori mail
```
sudo nano /etc/postfix/main.cf
```
> ![image](https://github.com/diotriandika/learn-networking/assets/109568349/3ae25d5c-0ff5-471b-b827-de84a592f118)

Reconfigure Postfix dengan command :
```
sudo dpkg-reconfigure postfix
```
Untuk menu awal hingga menu domain, kita samakan dengan sebelumnya. Disini kita juga bisa merubah konfigurasi yang kita lakukan sebelumnya jika dirasa ada yang salah/sekedar ingin merubah.

Untuk di menu postmaster recipient dikosongkan saja
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/fdc2f859-2612-468d-b9dc-c1409db3f8f3)

Menu selanjutnya kita bisa menambahkan accept mail dari domain lain atau server lain. Untuk menu Force Synchronous pilih NO.
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/50946dad-e0e7-4f67-8475-d4b6d1f75dbc)
>
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/76365a38-e0b0-49ad-bff0-daef45fa9c31)

Menu Local Network, masukan semua Network Local yang ingin diberikan akses. Disini saya menggunakan 0.0.0.0/0 yang bertujuan untuk memberikan akses kesemua network yang terhubung oleh server saya.
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/02c2a1de-c39c-42c8-b27b-5dcfc87c2d75)

Untuk mailbox size limit, `0` disini berarti tidak dibatasi.
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/3acf58db-d038-461b-b29c-ba6937e09cf3)

Menu ini yaitu memberikan ekstensi pada Local Address yang bertujuan untuk mendefiniskan setiap network. Disini kosongin aja
![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/e149f3e1-a89b-43e4-bd37-1bad8ba1ada5)

Terakhir menentukan protokol IP mana yang akan digunakan. Karena saya hanya menggunakan IPv4, oleh karena itu saya memiliih opsi IPv4 lalu OK
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/c0663be2-1d69-4c69-bd7e-134e42f78ce8)

> Note : Jika ada salah konfigurasi, gunakan `dpkg-reconfigure <service>` untuk konfigurasi ulang

Terakhir Restart Service Postfix
```
sudo systemctl restart postfix
```

## Verifikasi
Untuk meverifikasi apakah Postfix sudah berjalan, pertama kita buat user terlebih dahulu sebagai testing. **(Opsional)**
```
sudo adduser <user>
```
Disini saya membuat dua user sebagai test (nantinya mona akan sebagai penerima di task berikutnya)
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/658e80e2-11b8-4817-8ef3-f37dc2f2df34)

Mengirim pesan dengan telnet. Untuk mengirim pesan menggunakan telnet, disini menggunakan port 25
```
telnet <your-domain> 25
```
Format :
```
mail from: user_sender
rcpt to: user_receiver
data
<messages>
.
quit
```
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/3e8f1227-d2ef-46f3-8bd6-4cf2da6cc661)

