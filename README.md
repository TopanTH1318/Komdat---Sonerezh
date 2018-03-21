<h1 align="center"><img src="https://www.dadall.info/data/images/logo/sonerezhlogo.png"></h1>

[Sekilas Tentang](#sekilas-tentang) | [Instalasi](#instalasi) | [Konfigurasi Umum](#konfigurasi-umum) | [Otomatisasi](#otomatisasi) | [Cara Pemakaian](#cara-pemakaian) | [Pembahasan](#pembahasan) | [Referensi](#referensi)
:---:|:---:|:---:|:---:|:---:|:---:|:---:



# Sekilas Tentang
[`^ Back to Top ^`](#)

**Sonerezh** adalah sebuah CMS (*content management system*) aplikasi web untuk *streaming* lagu pribadi yang disimpan dan *Open Source*. Aplikasi ini digunakan untuk menyimpan lagu-lagu pribadi dan dapat diputar melalui browser yang kita miliki. Keunggulan dari **Sonerezh** adalah kita dapat *men-share* lagu-lagu pribadi kita ke beberapa orang yang kita izinkan untuk memainkan lagu-lagu yang tersebut dengan cara memberi perizinan melalui admin. Motto yang ditawarkan **Sonerezh** adalah `My Music Everywhere` yang diterapkan menyimpan lagu pribadi yang dapat dimainkan dimana saja dan siapa saja yang kita izinkan dapat mendengar musik pribadi kita.  

# Instalasi
[`^ Back to Top  ^`](#)

#### Komponen yang Dibutuhkan :
- Unix, Linux atau Windows.
- Apache Virtual Machine
- PHP
- MySQL
- Libav (optional)
- Cloud Commander (optional)
- PuTTY (optional)

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
1. Jalankan perintah untuk menjalankan **Cloud Commander**

	```
	$ cloudcmd
	```

2. Masuk ke halaman **Cloud Commander** menggunakan link [http://localhost:7000](http://localhost:7000).

![Cloud Commander](https://i.pinimg.com/originals/cb/21/36/cb2136898fffa6c8832c8eb71a18b830.png)
    
3. Navigasikan ke dalam folder root musik **Sonerezh**.

![Navigasi](https://i.pinimg.com/originals/2e/80/ce/2e80ce2fbcfa6c51b0b31852020462fa.png)

4. *Drag and Drop* file/folder musik dari komputer anda ke dalam **Cloud Commander**.

![DragNDrop](https://i.pinimg.com/originals/6e/9f/82/6e9f8299ae2dc82d14842900abe0faec.png)

5. Setelah itu masuk ke halaman setting pada **Sonerezh**, pada bagian menu *Database Management* klik tombol *Database Update*.

![Detected](https://i.pinimg.com/originals/46/58/99/46589934b61362bdf87eb28fef50634b.png)

![Update](https://i.pinimg.com/originals/96/6e/f8/966ef840c65ac5baf1171053c1ee7b17.png)


# Otomatisasi
[`^ Back to Top ^`](#)



# Cara Pemakaian
[`^ Back to Top ^`](#)

Cara pemakaian **Sonerezh** cukup mudah. Berikut untuk lebih jelasnya :
1. Sebelum menggunakan sonerezh, kita perlu login pada halaman.

    ![login]()

2. Setelah login, kita akan masuk ke halaman *homepage*. Pada halaman ini kita dapat memilih lagu atau album yang ingin dimainkan. lalu kita dapat membuat playlist sendiri dari lagu-lagu yanng ada di sonerezh.

    ![mainpage]()

3. Menu **Artist** digunakan untuk mencari dan mengurutkan lagu berdasarkan artist/penyanyi. Menu **Album** digunakan untuk mencari dan mengurutkan lagu berdasarkan album. Menu **Playlist** digunakan untuk membuka playlist yang pernah kita simpan sebelumnya.

    ![order]()

4. Menu **Database Update** berguna untuk memperbaharui lagu di sonerezh jika ada lagu yang ditambahkan oleh admin.

    ![catalog]()

5. Menu **Settings** berguna untuk mengatur konfigurasi sonerezh.

    ![customer]()

6. Menu **Users** berguna untuk mengatur akun sonerezh.

    ![cs]()

7. Untuk mengisi, menghapus dan meng-update lagu, yang bisa melakukannya hanya user dengan role administator. untuk melakukannya, diperlukan aplikasi pihak ketiga yaitu **cloud commander**. pada cloud commander, kita dapat  melakukan drag and drop musik yang diinginkan. user perlu melakukan update database jika ada perubahan pada database musik.

    ![stat]()


# Pembahasan
[`^ Back to Top ^`](#)

**Sonerezh** Merupakan suatu apliksasi audio streaming berbasis web dan dapat digunakan pada web browser apa saja. Aplikasi ini ditulis dalam bahasa pemrograman `PHP` yang support penggunaan `MySQL`. Berikut kelebihan dan kekurangan sonerezh:

**KELEBIHAN**
1.

**KEKURANGAN**
1.


# Referensi
[`^ Back to Top ^`](#)
1. [About Sonerezh](https://www.sonerezh.bzh/docs/en/introduction.html) - Sonerezh documentation
2. [How to Install Sonerezh on Ubuntu 16.04](https://www.sonerezh.bzh/docs/en/installation.html#installation-example-on-ubuntu-server) - Sonerezh installation

