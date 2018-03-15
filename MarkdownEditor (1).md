<h1 align="center"><img src="https://www.dadall.info/data/images/logo/sonerezhlogo.png"></h1>

[Sekilas Tentang](#sekilas-tentang) | [Instalasi](#instalasi) | [Konfigurasi](#konfigurasi) | [Otomatisasi](#otomatisasi) | [Cara Pemakaian](#cara-pemakaian) | [Pembahasan](#pembahasan) | [Referensi](#referensi)
:---:|:---:|:---:|:---:|:---:|:---:|:---:



# Sekilas Tentang
[`^ Back to Top ^`](#)

**Sonerezh** adalah sebuah CMS (*content management system*) aplikasi web untuk *streaming* lagu pribadi yang disimpans dan *Open Source*. Aplikasi ini digunakan untuk menyimpan lagu-lagu pribadi dan dapat diputar melalui browser yang kita miliki. Keunggulan dari **Sonerezh** adalah kita dapat *men-share* lagu-lagu pribadi kita ke beberapa orang yang kita izinkan untuk memainkan lagu-lagu yang tersebut dengan cara memberi perizinan melalui admin. Motto yang ditawarkan **Sonerezh** adalah `My Music Everywhere` yang diterapkan menyimpan lagu pribadi yang  dapat dimainkan dimana saja dan siapa saja yang kita izinkan.  

# Instalasi
[`^ Back to Top  ^`](#)

#### Kebutuhan Sistem :
- Unix, Linux atau Windows.
- 

#### Proses Instalasi :
1. Install SSH ke dalam server di *Virtual Machine*.
    ```
    $ sudo apt update
    $ sudo apt install ssh
    ```

2. Akses VM secara *remote* dengan cara buka terminal di *host* untuk login *remote* ke port 2222.
	```
    ssh <Username>@localhost -p 2222
	```
    
3. Pastikan seluruh paket sistem kita *up-to-date*, dan install seluruh kebutuhan sistem seperti `Apache`, `PHP`, dan `MySQL`.
    ```
    $ sudo apt update
    $ sudo apt install apache2
    $ sudo apt install mysql-server
    $ sudo apt install php
    $ sudo apt install libapache2-mod-php
    $ sudo apt install php-mysql
    $ sudo apt install php-gd
    $ sudo apt install libav-tools
    ```

4. Pastikan *ekstensi* **PHP** yang dibutuhkan sudah diaktifkan di `/etc/php/7.0/apache2/php.ini.`
 	```
    extension=exif.so
	extension=gd.so
	extension=pdo_mysql.so
    ```

5. Setelah semua kebutuhan selesai kita harus *men-restart* VM yang sebelumnya telah dibuat.
	```
    sudo service apache2 restart
    ```
    
6. Unduh **Sonerezh** ke dalam direktori kita dengan menggunakan *Git*
	```
    sudo apt-get install git
    
    cd /var/www/html/
    sudo git clone --branch master http://github.com/Sonerezh/sonerezh.git
    ```

7. Ubah otorisasi kepemilikan ke user www-data (webserver)
    ```
    $ sudo chown -R www-data: sonerezh/ 
    ```

8. Buat database dan user untuk **Sonerezh**.
    ```
    $ mysql -u root -p
        CREATE DATABASE sonerezh;
		GRANT ALL PRIVILEGES ON sonerezh.* TO '<username>'@'localhost' IDENTIFIED BY '<your password>';
		FLUSH PRIVILEGES;
		exit;
    ```

9. Konfigurasi Apache web server.
    ```
    $ sudo a2enmod rewrite
    $ sudo nano /etc/apache2/sites-available/sonerezh.conf
    
    # Ubah "<www.domain.com or IP-Address>" di isi dengan nama domain atau ip server :
		
       <VirtualHost *:80>
    		ServerName      <www.domain.com/IP-Address>
    		DocumentRoot    /var/www/html/sonerezh

    		<Directory /var/www/html/sonerezh>
        		Options -Indexes
        		AllowOverride All
        		<IfModule mod_authz_core.c>
            		Require all granted
        		</IfModule>
    		</Directory>

    		CustomLog   /var/log/apache2/sonerezh-access.log "Combined"
    		ErrorLog    /var/log/apache2/sonerezh-error.log
		</VirtualHost>
    ```

10. Simpan file, aktifkan Virtual Host yang baru dan *restart* Apache Web Server.
 
    ```
    $ sudo a2ensite sonerezh && sudo service apache2 restart
    ```

11. Kunjungi `http://<host>:<port>/sonerezh` untuk meneruskan instalasi.
    - Cek semua *requirements* apakah sudah terinstall semua atau tidak

      ![1](https://i.pinimg.com/originals/a9/8a/18/a98a18aafdddfdd864a63c581f353ee1.png)

    - Isi konfigurasi database sesuai kebutuhan

      ![2](https://i.pinimg.com/originals/ee/c7/f3/eec7f316da531c8cdf5169838b5e0014.png)

    - Isi informasi yang dibutuhkan untuk admin dan lokasi folder untuk musik

      ![3](https://i.pinimg.com/originals/5a/a8/33/5aa833a2596e6b491f45fb9f345b0a1f.png)
      
    - Setelah selesai maka akan masuk ke halaman *login* dan aplikasi sudah dapat dipakai.
      
      ![4](https://i.pinimg.com/originals/e4/72/cc/e472cc67ebffde5eff904e1025f222f6.png)

12. untuk *management* konten musik di server via web browser dapat menggunakan **Cloud Commander**.
    ```
    $ cd ~
	$ sudo apt install curl
	$ curl -sL https://deb.nodesource.com/setup_8.x -o nodesource_setup.sh
	$ sudo bash nodesource_setup.sh
	$ sudo apt install nodejs
	$ sudo apt install build-essential
    
	$ sudo npm i cloudcmd -g
	```
    
# Konfigurasi Umum
[`^ Back to Top ^`](#)

Untuk konfigurasi umum aplikasi **Sonerezh** dapat ditemukan pada halaman *Settings*. Berbagai macam konfigurasi dapat dilakukan di dalam halaman *Settings*, yaitu: 
- Mengubah atau menambah *path directory*
- Mengubah konversi format file music agar dapat dibaca oleh **Sonerezh**
- Memanajemen database yang digunakan **Sonerezh**
- Melihat statistik *library* atau berapa banyak *storage* yang telah dipakai oleh *caches* 
    
 ![settings](https://i.pinimg.com/originals/b1/f7/a4/b1f7a48225df6367a478d417728b06d8.png)

# Maintenance Database Musik
[`^ Back to Top ^`](#)

Ketika ingin *meng-update* musik agar lebih mudah dapat menggunakan aplikasi berbasis web yaitu **Cloud Commander** dengan cara:
1. Masuk ke halaman **Cloud Commander** menggunakan link [http://localhost:7000](http://localhost:7000).

![Cloud Commander](https://i.pinimg.com/originals/cb/21/36/cb2136898fffa6c8832c8eb71a18b830.png)
    
2. Navigasikan ke dalam folder root musik **Sonerezh**.

![Navigasi](https://i.pinimg.com/originals/2e/80/ce/2e80ce2fbcfa6c51b0b31852020462fa.png)

3. *Drag and Drop* file/folder musik dari komputer anda ke dalam **Cloud Commander**.

![DragNDrop](https://i.pinimg.com/originals/6e/9f/82/6e9f8299ae2dc82d14842900abe0faec.png)

4. Setelah itu masuk ke halaman setting pada **Sonerezh**, pada bagian menu *Database Management* klik tombol *Database Update*.

![Detected](https://i.pinimg.com/originals/46/58/99/46589934b61362bdf87eb28fef50634b.png)

![Update](https://i.pinimg.com/originals/96/6e/f8/966ef840c65ac5baf1171053c1ee7b17.png)


# Otomatisasi
[`^ Back to Top ^`](#)

Jika kalian masih merasa kesulitan dalam meng-install **Prestashop**, terdapat dua cara alternatif yang lebih mudah. Cara pertama adalah dengan menggunakan `script shell` yang otomatis akan menjalankan semua perintah instalasi pada terminal. Contoh `script shell` yang dapat kita gunakan adalah [setup.sh](../master/setup.sh)

Cara kedua adalah dengan menggunakan layanan yang tersedia pada *web-hosting provider*. Dengan layanan tersebut kita hanya perlu satu kali klik untuk meng-install **Prestashop**. Berikut langkah-lankah untuk melakukannya :
1. kita perlu mengunjungi *web-hosting provider* yang menyediakan *script* instalasi **prestashop** otomatis, seperti [SimpleScripts](http://www.simplescripts.com/script_details/install:PrestaShop), [Installatron](http://installatron.com/prestashop), atau [Softaculous](http://www.softaculous.com/apps/ecommerce/PrestaShop).
2. Sebagai contoh, kita akan menggunakan layanan dari [Installatron](http://installatron.com/prestashop). Kunjungi link tersebut lalu klik tombol **Install this Application**.

    ![Installatron](https://4.bp.blogspot.com/-PGjmovGOoOc/WNgQDHbE1RI/AAAAAAAAGk0/90dTTmH15cY6WSWqr8UU8BPETQs4KyxnACLcB/s1600/Screenshot_8.jpg)

3. Isi semua informasi yang dibutuhkan, lalu klik tombol **Install**.

    ![form](https://4.bp.blogspot.com/-5UwbsHAaBe0/WNgQDDjFdhI/AAAAAAAAGk4/coOLiqqP2DcVxq-hHwFa9cVW3P_t6p1tQCLcB/s1600/ss2.png)

4. Tunggu hingga proses instalasi selesai.



# Cara Pemakaian
[`^ kembali ke atas ^`](#)

Cara pemakaian **CMS Prestashop** ini sangat mudah, karena aplikasi ini telah menyediakan *interface* yang mudah dimengerti. Berikut untuk lebih jelasnya :
1. Sebelum menggunakan prestashop, kita perlu login pada halaman admin toko kita.

    ![login](https://4.bp.blogspot.com/-rmIdzrb4t4E/WOGbktO4p8I/AAAAAAAAGlc/z1NraShhrpUt-4Z0MVfXofZ5IV9hR_XbwCLcB/s1600/presta1.png)

2. Setelah login, kita akan masuk ke halaman *Dashboard*. Disini kita dapat melihat laporan penjualan website kita baik harian, mingguan, bulanan, bahkan tahunan.

    ![mainpage](https://3.bp.blogspot.com/-AjB_6mJZCxc/WOGbk3XPniI/AAAAAAAAGlg/NGZ_vOSF1s4jvw9Yx9jh7odGt8B4qHSuwCLcB/s1600/presta2.png)

3. Pada bagian samping kiri, terdapat berbagai menu yang dapat kita gunakan. Menu **Order** berguna untuk mengetahui informasi lebih detail tentang penjualan pada website kita. Disini kita dapat mencetak *invoices, credit slips, delivery slips*, dan lain-lain.

    ![order](https://1.bp.blogspot.com/-0-MhQjYMGvQ/WOGbkmpYSXI/AAAAAAAAGlY/JSwla_Py4ckrxc839Im9JKtyJjsmye_hQCLcB/s1600/presta3.png)

4. Menu **Catalog** berguna untuk mengetahui informasi lebih detail tentang barang apa saja yang dijual pada website kita, kategori apa saja yang ada, mengawasi stok barang yang tersisa, merek apa saja yang ada, melihat daftar diskon, dan lain-lain.

    ![catalog](https://4.bp.blogspot.com/-PQTBdZzcMBY/WOGblDw9tYI/AAAAAAAAGlk/GDC7cEzfWmo9LZ8bqcqwnv7iilAYlXNhwCLcB/s1600/presta4.png)

5. Menu **Customers** berguna untuk melihat informasi lebih detail tentang daftar pelanggan kita dan alamatnya.

    ![customer](https://2.bp.blogspot.com/-Mtss0c3rblo/WOGblcDU_bI/AAAAAAAAGlo/u1Hv6LWzwtAEuk9_NPQHnedI6PnotkEMwCLcB/s1600/presta5.png)

6. Menu **Customer Service** berguna untuk mengatur hubungan dengan pelanggan, seperti menerima keluhan, mengirim pesan kepada pelanggan, pengembalian barang, dan lain-lain.

    ![cs](https://3.bp.blogspot.com/-ZlT_WQ0bpyk/WOGblYW-yPI/AAAAAAAAGls/gcZb8k36rEUn8dDFnGXl35R_Uhy_lNMeACLcB/s1600/presta6.png)

7. Menu **Stat** berguna untuk melihat informasi lebih detail dari website kita, seperti jumlah pengunjung setiap harinya, lokasi pelanggan terbanyak, barang apa yang populer, dan lain-lain.

    ![stat](https://3.bp.blogspot.com/-kvZ9MMH6Fxc/WOGblsk5jpI/AAAAAAAAGlw/p8LhO5LIvMU4WnDEF5NKg7--tKy8mWa-wCLcB/s1600/presta7.png)

8. Selain menu-menu yang berhubungan dengan penjualan, **Prestashop** juga menyediakan menu untuk meningkatkan performa website kita seperti menu untuk menginstal modul atau plugin, menu untuk memperindah tampilan website, menu untuk mengatur pengiriman dan pembayaran barang, bahkan menu lokalisasi untuk meningkatkan layanan pada region tertentu.

    ![improve](https://3.bp.blogspot.com/-d_DufFiAblk/WOGbls1KYxI/AAAAAAAAGl0/AsyvLt1zp1IP1BG1PbDaG91PJKV-xDkhACLcB/s1600/presta8.png)

9. Selain itu, terdapat juga menu untuk mempermudah konfigurasi website kita baik itu konfigurasi umum maupun konfigurasi lanjut.

    ![configure](https://4.bp.blogspot.com/-58ke7QyDUwQ/WOGbmCZvHFI/AAAAAAAAGl4/LuQRv6uYmywWjbzmJy82UwNu5qCg8fp1gCLcB/s1600/presta9.png)



# Pembahasan
[`^ kembali ke atas ^`](#)

**Prestashop** ditulis dalam bahasa pemrograman `PHP` yang support untuk penggunaan MySQL. Sebagai salah satu CMS yang paling banyak digunakan di dunia, aplikasi ini menawarkan berbagai kelebihan, diantaranya :
- Aplikasi memiliki panel administrasinya mudah digunakan dan fleksibel, sehingga dapat disesuaikan dengan kebutuhan.
- Mendukung berbagai layanan pembayaran utama, seperti `PayPal`, `VISA`, `MasterCard`, dan `Maestro`.
- Diterjemahkan dalam banyak bahasa, termasuk Bahasa Indonesia.
- Memiliki desain yang *responsive*, sehingga dapat dibuka menggunakan *device* apapun.
- Memiliki lebih dari tiga ratus fitur untuk memudahkan pengguna.
- Banyak pengguna yang berkontribusi pada *discussion boards* dan sejenisnya, sehingga masalah yang dihadapi pengguna dapat cepat terselesaikan.

Tentu saja, sebuah aplikasi pasti memiliki kekurangan. Kekurangan yang dimiliki **Prestashop** antara lain :
- Penggunaan fitur atau modul yang lengkap menyebakan proses loading dari aplikasi ini menjadi sangat lambat
- Penggunaan *resource* memory aplikasi ini cukup besar, terutama ketika menggunakan fitur atau modul yang lengkap.
- Sebagian besar modul dan tema yang tersedia tidak gratis.

Jika dibandingkan dengan CMS sejenisnya seperti **Microweber**, CMS ini memiliki beberapa keunggulan dan kelemahan. Berikut adalah beberapa perbandingan antara kedua CMS ini :
- **Microweber** menyediakan proses design yang fleksibel dengan fitur *Drag and Drop* tanpa batasan, sehingga pengguna bebas mengkreasikan tampilan websitenya. Sedangkan **Prestashop** hanya menyediakan fitur design berupa penggantian template dan logo, adapun template yang tersedia tidak gratis.
- Modul atau plugin yang tersedia pada **Prestashop** jauh lebih banyak dibandingkan pada **Microweber**.
- **Prestashop** memiliki pengguna yang jauh lebih banyak daripada **Microweber** yang aktif pada forum-forum diskusi untuk membantu pengguna pemula.
- **Microweber** lebih ringan daripada **Prestashop** karena modulnya yang sedikit.
- Proses instalasi **Prestashop** lebih mudah karena berbasis PHP saja, sedangkan **Microweber** menggunakan framework laravel sehingga proses instalasi lebih sulit, terutama dalam hal *dependency*.



# Referensi
[`^ kembali ke atas ^`](#)

1. [About PrestaShop](https://www.prestashop.com/) - PrestaShop
2. [How to Log Into a VPS with PuTTY on Windows](https://www.digitalocean.com/community/tutorials/how-to-log-into-a-vps-with-putty-windows-users) - DigitalOcean
3. [How to Install PrestaShop on Ubuntu 16.04](http://idroot.net/linux/install-prestashop-ubuntu-16-04/) - idroot
4. [One Click Install PrestaShop](https://www.prestashop.com/blog/en/how-to-install-prestashop/) - PrestaShop
5. [PrestaShop Review](http://whichshoppingcart.com/prestashop.html) - whishshoppingcart
