# Langkah - Langkah Mengganti Hostname
Hostname sebuah perangkat bisa dikatakan nama/alias dari perangkat tersebut.
## Mengganti Hostname melalui /etc/hostname
Masuk User ROOT

Edit file `/etc/hostname` kemudian rename hostname sebelumnya dengan hostname yang diinginkan

    nano /etc/hostname

Reboot system menggunakan

    systemctl reboot

Jika terjadi error seperti dibawah

[![image](https://github.com/diotriandika/learn-networking/assets/109568349/c97ad4b9-a161-4df0-980f-90b2b891b273)](https://github-production-user-asset-6210df.s3.amazonaws.com/109568349/263467903-c97ad4b9-a161-4df0-980f-90b2b891b273.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20230907%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230907T024605Z&X-Amz-Expires=300&X-Amz-Signature=4cc2bb20495e366df44f06c800319583382a60f4c84d0d21a1aba9360ae1d45a&X-Amz-SignedHeaders=host&actor_id=109568349&key_id=0&repo_id=676416420)

Edit file `/etc/hosts` dan rename default hostname dengan hostname yang baru atau tambahkan line baru dibawah kemudian save & exit

    nano /etc/hosts

> Default
> 
> ![image](https://github.com/diotriandika/learn-networking/assets/109568349/4490a06c-50ca-40a0-a796-f41af54ba02f)
>
> Set dengan format `127.0.1.1 {{fqdn}} {{hostname}}`
>
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/f3b73599-fadb-401e-aeb9-07f24b4b7231)
>
> Atau
>
> ![image](https://github.com/diotriandika/learn-networking/assets/109568349/a4fcac3f-5f11-4911-93ac-9cf715c52083)
 
## Mengganti Hostname menggunakan hostnamectl
Gunakan program `hostnamectl` untuk mengganti hostname

    hostnamectl set-hostname <new-hostname>

Edit file `/etc/hosts` dan tambahkan `127.0.0.1 <hostname>` dibawah default hostname

    nano /etc/hosts

## Verifikasi
Untuk melihat apakah hostname sudah berhasil diubah atau belum, bisa menggunakan command `hostname`, re-login atau juga bisa langsung reboot system

## Troubleshoot
Jika hostname yang diset di `/etc/hosts` hilang, set permanent dengan mengubah konfigurasi di `/etc/cloud/templates/hosts.debian.tmpl` dan set line ini

> replace {{fqdn}} dengan qualified domain name kalian, atau jika bingung apa itu bisa replace dengan hostname kalian saja
> 
> replace {{hostname}} dengan hostname kalian
>
> ![image](https://github.com/diotriandika/lnearher-public-repository/assets/109568349/02b2bb47-4f18-4494-ace3-2dcdbf8a29db)
>
> Atau jika bingung bisa tambahkan line baru seperti yang diatas dengan format `127.0.0.1 <your-hostname>`
