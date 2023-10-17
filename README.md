# Laporan Resmi Modul 2 Jarkom 2023

Kelompok **B15**

Anggota:

- [MH] Muhammad Hidayat (05111940000131)
- [VG] Victor Gustinova (5025211159)

## 1. Topology

Untuk setting awal maka dibuat topologi sesuai plottingan (topologi 7)

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/a1b5433e-4a7e-48f6-a295-10fad9d037db)

## 2. Arjuna Domain
Dibuat pengaturan domain arjuna pada yudhistira. Pertama yudhistira perlu melakukan update dan install bind9.

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/fb762393-ce1d-4376-a86d-b8b3699550a5)

Setelah itu domain arjuna bisa dikonfigurasi sebagai berikut.

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/392d1b77-e693-4623-8618-cf7d1857fdb4)

lakukan echo

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/84264160-2f70-464d-851d-7f30da7e14fe)


## 3 dan 4 (+ setting domain no 7). Abimanyu Domain dan Parikesti Subdomain
Setting pada domain abimanyu juga dilakukan hal yang sama. Untuk konfigurasi no 4 maka ditambahkan parikesit IN A.
![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/1f7ac10e-900b-4a64-9650-55e37b665eaf)

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/dc17ba72-194e-4ce0-a2ae-4614779bb8c8)

Untuk nomor 7 akan dilakukan delegasi sehingga perlu setting ns1 IN A serta baratayuda In NS. 

## 5. Abimanyu Reverse Domain
Reverse DNS dilakukan dengan membuat file in-addr.arpa untuk Ip yang diinginkan.

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/7263adfc-db45-4040-8ac6-2725acddac65)

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/f6110413-3fd5-4994-9a6c-106c2ee269f0)

2.16.10.in-addr.arpa menunjukkan 3 nomor awal dari abimanyu yang direverse dan 3 IN NS PTR menunjukkan nomor IP terakhir. 

## 6. Werkudara DNS Slave
Pada Yudhistira perlu dilakukan zone transfer untuk setiap domain yang akan dipakai juga oleh slave.

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/d14b7b88-3226-4ec8-af39-ca8a40c7af3d)

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/3e7b926c-5b0f-4f7c-b938-a7ed19ec1292)

Pada werkudara dilakukan instalasi bind9.

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/bf2b6905-0caf-4076-b76f-1cb3461d678d)

Setelah itu melanjutkan zone transfernya.

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/e2534c59-793a-4768-8a2f-9d89cf9a706b)

Pada gambar diatas terdapat allow-transfer. Konfigurasi tersebut akan memperbolehkan delegasi subdomain yang akan dilakukan pada nomo 7.

## 7 dan 8. Baratayuda Subdomain Delegate dan RJP Sub-Subdomain Delegate
Dilakukan pengeditan file named.conf.options pada kedua node DNS agar menjadi seperti berikut.

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/0b846198-3c7c-449a-b25d-b5fb5cd21999)

Lalu pada Werkudara lakukan konfigurasi domain untuk baratayuda.abimanyu

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/91e1a187-8676-4cf5-a015-ac2e351ede82)

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/a5c71902-5641-4322-8f0c-55ffc8754aea)

Pada gambar konfigurasi file bind baratayuda diatas terdapat rjp IN A dan www.rjp IN CNAME. KOnfigurasi tersebut akan membuat subdomain untuk rjp.baratayuda.abimanyu dan www.rjp.baratayuda.abimanyu.

## 9 dan 10. Arjuna Load Balancer 
Pada arjuna dilakukan installasi bind9 dan nginx.

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/2067d350-e313-491f-b126-7827722b66e0)

Setelah itu dilakukan konfigurasi load balancer round robin pada file lb-jarkom

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/f7abce64-89ea-4788-af60-fcb45a0fac5d)

Untuk konfigurasi no 10, ditambahkan port yang diinginkan disebelah ip webserver yang dipakai.

Pada setiap web server, dilakukan instalasi nginx php-fpm dan dibuat file php sederhana untuk mengetahui letak web yang diakses client.

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/0dea691c-d9d9-4f23-a66c-edf09bcb39f3)

Selanjutnya dilakukan konfigurasi sites-available sebagai berikut. Untuk webserver berbeda, perbedaan hanya terdapat pada konfigurasi listen.
```
echo '
 server {

        listen 8002;

        root /var/www/jarkom/;

        index index.php index.html index.htm;
        server_name _;

        location / {
                        try_files $uri $uri/ /index.php?$query_string;
        }

        # pass PHP scripts to FastCGI server
        location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
        }

 location ~ /\.ht {
                        deny all;
        }

        error_log /var/log/nginx/jarkom_error.log;
        access_log /var/log/nginx/jarkom_access.log;
 }' > /etc/nginx/sites-available/jarkom

ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled

service php7.2-fpm start
service php7.2-fpm restart
service nginx restart
```

## 11 dan 12. Abimanyu Web Server
Pada abimanyu dilakukan instalasi apache2 dan resource yang diperlukan.

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/a2f2f5c3-8135-4719-ba17-faebf9ec531f)

Setelah itu file .conf akan diubah menjadi sebagai berikut.

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/2551d4da-91d5-4973-957a-6488da7610de)

Perubahan URL index.php/home manjadi /home dilakukan menggunakan alias.

Jangan lupa untuk menjalankan
```a2ensite abimanyu.b15.com.conf```

## 13, 14, 15, dan 16. Parikesit Website
Berikut merupakan setting subdomain parikesit. Resource di download dan dipindahkan ke directory yang dipakai.

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/9ba3374b-1c89-4043-b8b9-edef70e71ff0)

Pada file parikesit.abimanyu.b15.com.conf dilakukan konfigurasi sebagai berikut.

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/17cb7cd6-a2a9-4402-92c9-9c3c2c129684)

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/96947ebe-38dc-460e-a714-5d30b0a9fdb5)

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/8eceb28e-f81f-46b5-83a4-8e2c27a19178)

Untuk menggunakan directory listing maka dilakukan setting <Directory> dengan isinya Options +Indexes (permintaan diterima) atau -Indexes (permintaan ditolak). Selanjutnya kustomisasi error code dilakukan dengan memilih file untuk ErrorDocument yang akan mengarahkan client jika error ditemui. Agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi 
www.parikesit.abimanyu.yyy.com/js maka dilakukan ```Alias```.


## 17. RJP Port Limiting
Setting port dilakukan dengan mengubah VirtualHost Port pada file rjp.baratayuda.abimanyu.b15.com.conf

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/6b79f674-c45d-437b-8dcc-307641a7aea2)

## 18. RJP Authentication
Autentikasi dilakukan juga dengan mengatur file rjp.baratayuda.abimanyu.b15.com.conf.

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/0973f710-f1a3-4e04-837c-f457817ecb35)

Selanjutnya ditambahkan informasi user dan password

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/202ff3da-f07f-41b2-a43b-ed3174dd470a)

## 19. Abimanyu WWW alias
Untuk mengalihkan secara automatis maka ditambahkan redirect pada file 000-default.conf

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/7daf1cb3-b031-4011-9d78-2a100ab09fac)


## 20. Redirect Matching Images
Digunakan mod rewrite dengan konfigurasi 
```
RewriteEngine On
RewriteCond %{REQUEST_URI} ^/public/images/(.*)abimanyu(.*)
RewriteCond %{REQUEST_URI} !/public/images/abimanyu.png
RewriteRule abimanyu http://parikesit.abimanyu.b15.com/public/images/abimanyu.png$1 [L,R=301]'
```

pada file /var/www/parikesit.abimanyu.b15/.htaccess

juga perlu ditambahkan pengaturan <Directory> pada parikesit.abimanyu.b15.conf seperti berikut.

![image](https://github.com/VictorGstn/Jarkom-Modul-2-B15-2023/assets/125529445/89c2a70a-f6b6-4dfa-a969-1aef08427eb2)


## Permasalahan saat mengerjakan
1. Kurang berkomunikasi antar anggota kelompok
2. Error terkadang muncul setelah menutup dan membuka kembali project
3. Tidak terpikirkan untuk membedakan script .sh untuk setiap nomor sehingga semua code terdapat pada .bashrc dan menjadi sedikit lebih membingungkan
