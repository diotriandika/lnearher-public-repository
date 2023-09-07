# Langkah - Langkah Instalasi Roundcube Webmail
![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/948c05b9-0cd2-4cb7-87a4-25a04d0f27ab)

Roundcube merupakan sebuah email client IMAP yang berbasis Web atau bisa disebut Webmail yang digunakan untuk mengelola email. Karena Roundcube dioperasikan melalui Website, jadi pengoperasiannya mudah untuk mayoritas orang.

- Pastikan sudah mengkonfigurasi [Postfix](https://github.com/diotriandika/lnearher-public-repository/blob/7637ee47f57053b9c7ed268924883955839f3f41/ASJ-Linux/Mail-Server/Postfix-SMTP-Server.md) dan [Dovecot](https://github.com/diotriandika/lnearher-public-repository/blob/94516ce2920d75744dd8270a15a73f6419e2e732/ASJ-Linux/Mail-Server/Dovecot-IMAPD-POP3.md).
- Project ini terhubung dengan project sebelumnya agar mudah untuk mengerti fungsi dari setiap programnya.

## Instalasi Roundcube
Instal `mariadb-server` sebagai database untuk Roundcube dan rouncube itu sendiri.
```bash
sudo apt install mariadb-server roundcube
```
Untuk menu dibawah, select `yes` untuk membuat database secara otomatis oleh roundcube.
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/c369275b-cbd6-4640-8f6f-df23523ef6a8)S

Selanjutnya masukan password yang akan digunakan sebagai password database roundcube dan confirm password

## Konfigurasi Roundcube
Edit file konfigurasi `config.inc.php`
```bash
sudo nano -l /etc/roundcube/config.inc.php
```
Pada line `36` tambahkan domain dari mail server yang akan digunakan
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/98048f76-1a72-4c21-bcb6-73bb4a34976b)

Diline `48` ganti `'smtp_server'` dengan domain dari mail server, pada line `51` ganti `'smtp_port'` menjadi `22` dan kosongkan value dari `'smtp_user'` dan `'smtp_password'` pada line `55` dan `59`
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/018befc8-5c77-4efe-b6f9-cbe270e2e750)

Sekarang configure ulang roundcube
```bash
sudo dpkg-reconfigure roundcube-core
```
Untuk menu dibawah kita kosognkan saja, karena kita disini kita tidak menggunakan tls/ssl
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/e7a31b20-c2a8-4fe1-85f8-8acc43307456)

Selanjutnya pilih bahasa, dan menu reinstall pilih `No` jika tidak ingin reinstall database yang sudah dibuat sebelumnya
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/1faa457a-f704-4ca7-aac7-c0fd3123cc59)

Menu selanjutnya adalah memilih software webservice untuk menampilkan Webmail roundcube, karena kita memerlukan webserver sebagai perantara. Disini saya hanya memilih apache2 dan kemudian select `yes` pada menu restart webserver.
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/179ea5c0-1d9b-4ffc-9501-f463d338e0b5)
>
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/fe9847fd-6315-47fa-bcde-a6e7d28a0fe1)

Terakhir pilih `keep the local version currently installed` jika tidak ingin mengubah versi roundcube.
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/daa7110f-844f-44db-9504-1d2093016b2c)

## Konfigurasi Apache2 sebagai Webserver untuk Roundcube
Edit file `apache2.conf` dan tambahkan `Include /etc/roundcube/apache.conf` pada line paling akhir
```bash
sudo nano /etc/apache2/apache2.conf
```
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/8926aa21-e0c3-48cd-aa66-ac8cdc259ac5)

Selanjutnya masuk ke direktori `/etc/apache2/sites-available/` kemudian buat file yang nantinya digunakan sebagai aktivasi site roundcube
```bash
$ cd /etc/apache2/sites-available
$ sudo touch <your-site-name>.conf
```
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/fbf6dd9b-835e-4147-b180-bdc54a53e78f)

Edit file `<your-site-name>.conf` dan isi dengan input berikut
```bash
sudo nano <your-site-name>.conf
```
```bash
<VirtualHost *:80>
    ServerName <your-domain-name>
    DocumentRoot /usr/share/roundcube
</VirtualHost>
```
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/37f893b3-f4ab-474e-affe-55a8cf6b2498)

Disable site default dan enable site yang sudah dibuat barusan kemudian restart service apache2
```bash
$ sudo a2dissite 000-default.conf
$ sudo a2ensite <your-site-name>.conf
$ sudo systemctl restart apache2
```
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/f5bbcf6d-a8ff-4558-990a-5f92280ea7d7)


## Verifikasi
Akses domain mail server di browser client, pastikan DNS Mail server sudah diset di client tersebut.

Jika sudah maka akan muncul tampilan login, disini kita bisa masukan user yang sudah dibuat. Contohnya saya mau melihat email yang sebelumnya saya kirim menggunakan user `zeta` ke `moona`.
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/ae9d2669-5317-47d1-a7c0-4f2334fc71f6)

Nah disini sudah terlihat email yang sebelumnya sudah kita kirim melalui user `zeta` dan bisa dilihat isinya juga sama
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/ff959332-90f0-4d92-afb3-544c27bd2ce1)
>
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/c37b7610-b4b1-4bb4-9965-920e6f524b8f)

Kita juga bisa mengirim pesan kembali melalui Webmail ini, sebagai contoh saya mengirimkan email ke `zeta@itnsa.id` dari user `moona`
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/d56cefbd-c8b5-4332-943d-fdfe61a79ec6)
>
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/689588d4-1bb8-4930-a83f-fd7866c06de9)

> Note : Jika misalnya kalian tidak bisa mengakses websitenya, allow port 80 dengan `ufw` atau tools firewall lainnya, begitu juga dengan jalur udp yaitu 53, port 23, serta 110
>
> Contoh
> ```bash
> ufw allow 80
> ufw allow 35
> ufw allow 23
> ufw allow 110
> ufw reload
> ufw resatrt
> ```

**_Done_**

