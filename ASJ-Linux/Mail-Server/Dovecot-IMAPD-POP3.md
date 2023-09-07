# Langkah - Langkah Instalasi Dovecot IMAPD dan POP3
Dovecot adalah server IMAP & POP3 yang bertugas untuk menyimpan dan meneruskan email tersebut ke penerima dengan protokol yang IMAP & POP3.

> Berikut referensi hubungan antara Postfix dengan Dovecot : https://www.proweb.co.id/articles/postfix/postfix_dovecot.html

- Pastikan sudah melakukan konfigurasi dengan Postfix seperti halnya [disini](https://github.com/diotriandika/lnearher-public-repository/blob/7637ee47f57053b9c7ed268924883955839f3f41/ASJ-Linux/Mail-Server/Postfix-SMTP-Server.md)
## Instalasi Dovecot
Install Dovecot dengan IMAPD dan POP3
```
sudo apt install dovecot-imapd dovecot-pop3
```
## Konfigurasi Dovecot
Edit file konfigurasi `dovecot.conf` dan uncomment pada line ke 30
```
sudo nano -l /etc/dovecot/dovecot.conf
```
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/a427cd46-e047-45de-b661-a61ec36d0df1)

Edit file konfigurasi `10-auth.conf`, uncomment di line 10 dan ganti disable_plaintext_auth menjadi no
```
sudo nano -l /etc/dovecot/conf.d/10-auth.conf
```
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/8f48250e-5697-414b-a370-0c8ae6dc5341)

Edit file konfigurasi `10-mail.conf`, edit `mail_location` dengan `~/Maildir`
> 
Terakhir restart service dovecot
```
sudo systemctl restart dovecot
```
## Verifikasi
Untuk mengecek apakah mail yang kita kirim sebelumnya sudah bisa atau belum kita cek menggunakan telnet
```
telnet mail.itnsa.id 110
```
Format :
```
user <user-receiver>
pass <user-receiver-password>
list
retr <message number>
```
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/f1b97809-8656-458b-a9ef-cba1ae43b7e6)

Nah disini sudah bisa kita lihat isi pesan yang dikirim sebelumnya oleh `zeta@itnsa.id`
> Note : Jika tidak ada pesan masuk. Coba restart kedua service `postfix` dan `dovecot` lalu kirim pesan ulang seperti sebelumnya
