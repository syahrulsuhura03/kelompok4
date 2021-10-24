

## 				Laporan Hasil Praktikum Modul 1

Nama anggota :

wahid yasin 1202192064

syahrul suhura 1202190027

1. Rename ubuntu_php5.6 menjadi ubuntu_landing, serta rubah IP mengikuti skema yang baru

   pertama melakukan pengecekan menggunakan

   ```markdown
   sudo lxc-ls-f
   ```

   kemudian rename menjadi ubuntu_landing

   ```markdown
   sudo lxc-copy -R ubuntu_php5.6 -N Ubuntu_landing
   ```

   

![step1](https://user-images.githubusercontent.com/93044506/138595438-01010eeb-5912-44c7-9cc0-b3f86723654f.png)


​	kemudian setting ip static ubuntu_landing dengan ip addres 10.0.3.103

![step2](https://user-images.githubusercontent.com/93044506/138595452-6370f241-7ddf-451c-a786-935ee55d9ee4.png)

setelah setting kita reboot kemudian start ubuntu_landing dan masuk ke ubuntu_landing 

```markdown
sudo lxc-attach -n ubuntu_landing
```



![step3](https://user-images.githubusercontent.com/93044506/138595463-e2c56097-4aca-4ca6-8c30-c2b40e74dd7f.png)

 kemudian pengecekkan ip ubuntu_landing dengan ping google.com

```markdown
ping google.com
```

![step4](https://user-images.githubusercontent.com/93044506/138595470-382fef44-433e-4bfd-a7c0-77b1f970763f.png)




2. Install lxc debian 9 dengan nama debian_php5.6

   ````markdown
   sudo lxc-create -n debian_php5.6 -t download -- -- dist debian --release stretch --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
   ````

   
![image-20211024120722701](https://user-images.githubusercontent.com/93044506/138595653-28df44f2-2336-4fc1-9a6a-5785837f9366.png)

  setelah itu mengecek container sudah ter-instal atau belum menggunakan

````markdown
sudo lxc-ls -f
````
![image-20211024120912903](https://user-images.githubusercontent.com/93044506/138595662-d495ffde-fa88-4b1f-aa1e-a3cef12778a8.png)


3. kemudian di start debian_php5.6 dan masuk dalam debian_php5.6

````markdown
sudo lxc-start -n debian_php5.6 -d
sudo lxc-attach -n debian_php5.6
````

![step6](https://user-images.githubusercontent.com/93044506/138595741-fdeaf220-fd3c-45a8-91ed-06460dce31f7.png)

install net-tools

````markdown
apt install nano net-tools curl -y
````

![image-20211024121603205](https://user-images.githubusercontent.com/93044506/138595754-cdc318ce-0860-42e7-8cb6-c23166af8eeb.png)


 lakukan setting ip static menggunakan address 10.0.3.102

````markdown
set up static IP
````

kemudian setting nginx

````markdown
apt install nginx-extras -y
````

![image-20211024122519605](https://user-images.githubusercontent.com/93044506/138595798-70e0b3c3-96c5-43d2-a0bd-c099e6a174f3.png)


lakukan setting pada interface

````
cd /etc/nginx/sites-available
mv lxc_php5.6.dev lxc_landing.dev
nano lxc_landing.dev
cd ../sites-enabled
ln -s /etc/nginx/sites-available/lxc_landing.dev .
nginx -t


````

![image-20211024122836328](https://user-images.githubusercontent.com/93044506/138595823-44314910-8674-49fc-a4f7-018485230e20.png)

````markdown
nano /etc/hosts
````

![image-20211024125939915](https://user-images.githubusercontent.com/93044506/138595895-ba869851-b903-4903-8b2e-a3173274fbd1.png)


lakukan setting pada debian html php5.6

````markdown
/etc/nginx/sites-enabled# cd /var/www/html
mkdir lxc_php5.6
cp index.nginx-debian.html lxc_php5.6/index.html
cd lxc_php5.6/
nano index.html
````



![image-20211024124806360](C:\Users\COMPUTER\AppData\Roaming\Typora\typora-user-images\image-20211024124806360.png)

![image-20211024124811734](C:\Users\COMPUTER\AppData\Roaming\Typora\typora-user-images\image-20211024124811734.png)



melakukan pengecekkan apakah sudah terhubung atau belum

````markdown
curl -i http://lxc_php5.dev
````



![image-20211024130126876](C:\Users\COMPUTER\AppData\Roaming\Typora\typora-user-images\image-20211024130126876.png)



4. setup nginx pada ubuntu_landing untuk domain http://lxc_landing.dev , buat halaman index.html yang menerangkan informasi nama lxc

````markdown
sud0 lxc-attach -n ubuntu_landing
cd /etc/nginx/sites-avalaible/
nano lxc_php5.6.dev
````

![image-20211024130744115](C:\Users\COMPUTER\AppData\Roaming\Typora\typora-user-images\image-20211024130744115.png)

````markdown
cd ../site-enabled/
rm lxc_php5.6.dev
ln -s /etc/nginx/sites-avalaible/lxc_php5.6.dev
nginx -t
-s reload
nano /etc/hosts
````

![image-20211024131051224](C:\Users\COMPUTER\AppData\Roaming\Typora\typora-user-images\image-20211024131051224.png)

![image-20211024131056345](C:\Users\COMPUTER\AppData\Roaming\Typora\typora-user-images\image-20211024131056345.png)

lakukan setting pada ubuntu_landing html php5.6

````
/etc/nginx/sites-enabled# cd /var/www/html
mkdir lxc_php5.6
cp index.nginx-ubuntu_landing.html lxc_php5.6/index.html
cd lxc_php5.6/
nano index.html
````

![image-20211024131637715](C:\Users\COMPUTER\AppData\Roaming\Typora\typora-user-images\image-20211024131637715.png)



kemudian melakukan pengecekkan

![image-20211024131644482](C:\Users\COMPUTER\AppData\Roaming\Typora\typora-user-images\image-20211024131644482.png)

5. LXC ubuntu_landing harus auto start ketika vm dinyalakan, hal ini digunakan untuk menjaga agar website company profile tidak mengalami *downtime*

````markdown
lxc-ls-f
````

![image-20211024131916101](C:\Users\COMPUTER\AppData\Roaming\Typora\typora-user-images\image-20211024131916101.png)



6. setup nginx pada vm.local untuk mengatur *proxy_pass* dimana :

- mengakses [http://vm.local](http://vm.local/) akan diarahkan ke http://lxc_landing.dev
- mengakses http://vm.local/blog akan diarahkan ke http://lxc_php7.dev
- mengakses http://vm.local/app akan diartahkan ke http://lxc_php5.dev



````markdown
sudo nano /etc/hosts
cd /etc/nginx/sites-available
sudo nano vm.local
sudo nginx -t
sudo nginx -s reload
curl -i http://vm.local/

````

![image-20211024132610527](C:\Users\COMPUTER\AppData\Roaming\Typora\typora-user-images\image-20211024132610527.png)

![image-20211024132627110](C:\Users\COMPUTER\AppData\Roaming\Typora\typora-user-images\image-20211024132627110.png)

![image-20211024132656457](C:\Users\COMPUTER\AppData\Roaming\Typora\typora-user-images\image-20211024132656457.png)

![image-20211024132929489](C:\Users\COMPUTER\AppData\Roaming\Typora\typora-user-images\image-20211024132929489.png)

![image-20211024133139655](C:\Users\COMPUTER\AppData\Roaming\Typora\typora-user-images\image-20211024133139655.png)



7. untuk kebutuhan presentasi mereka, browser di laptop mereka harus dapat mengakses ketiga url tersebut

   

![image-20211024133358421](C:\Users\COMPUTER\AppData\Roaming\Typora\typora-user-images\image-20211024133358421.png)

![image-20211024133405035](C:\Users\COMPUTER\AppData\Roaming\Typora\typora-user-images\image-20211024133405035.png)

![image-20211024133411930](C:\Users\COMPUTER\AppData\Roaming\Typora\typora-user-images\image-20211024133411930.png)



8. Menyiapkan analisa untuk diserahkan ke CTO

   a. mengapa untuk kebutuhan php5.6 tidak bisa menggunakan ubuntu 16.04, sehingga perlu diganti os ke debian 9?

   b. kenapa harus menggunakan virtualisasi LXC pada skema website yang akan didevelop

   c. apa yang dimaksud dengan proxy server? kenapa vm.local bisa kita anggap sebagai proxy server?



a. karena pada ubuntu 16.04 php7 adalah sebagai standart untuk menggunakan ubuntunya, dan php5 tidak termasuk dalam standart ubuntu 16.04. jadi ketika ingin menggunakan php5.6 lebih baik mengganti os ke debian 9

b.   Linux Containers adalah metode virtualisasi tingkat sistem operasi untuk menjalankan beberapa sistem Linux terisolasi yang disebut wadah pada satu Host kontrol. Karena LXC menyediakan virtualisasi tingkat sistem operasi, itu bukan melalui mesin virtual yang penuh, tetapi ia menyediakan lingkungan virtualnya sendiri yang memiliki proses dan ruang jaringan sendiri. dan juga karena pemakaian resource server yang jauh lebih ringan

c. proxy server adalah sebuah komputer server atau program komputer yang dapat bertindak sebagai komputer lainnya untuk melakukan request terhadap content dari Internet dapat menyimpan, sementara berupa cache file html server lain untuk mempercepat akses internet

Vm.local bisa dianggap sebagai proxy server karena  sebagai perantara antara client hosts, dengan server yang ingin diaksesnya. 
