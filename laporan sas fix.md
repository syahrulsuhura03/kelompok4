

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


![image-20211024124806360](https://user-images.githubusercontent.com/93044506/138596930-f77510c7-e65e-42d8-9179-34cae068b302.png)

![image-20211024124811734](https://user-images.githubusercontent.com/93044506/138596962-bee82ad8-5367-4d05-831e-f04b4ec0409a.png)




melakukan pengecekkan apakah sudah terhubung atau belum

````markdown
curl -i http://lxc_php5.dev
````



![image-20211024130126876](https://user-images.githubusercontent.com/93044506/138596974-3da2335d-5885-4b25-bbc9-4cc962131e63.png)




4. setup nginx pada ubuntu_landing untuk domain http://lxc_landing.dev , buat halaman index.html yang menerangkan informasi nama lxc

````markdown
sud0 lxc-attach -n ubuntu_landing
cd /etc/nginx/sites-avalaible/
nano lxc_php5.6.dev
````

![image-20211024130744115](https://user-images.githubusercontent.com/93044506/138596991-fb207d54-aaa5-4e82-b246-8955cbc15bdf.png)
)

````markdown
cd ../site-enabled/
rm lxc_php5.6.dev
ln -s /etc/nginx/sites-avalaible/lxc_php5.6.dev
nginx -t
-s reload
nano /etc/hosts
````

![image-20211024131051224](https://user-images.githubusercontent.com/93044506/138597015-f009560b-2d24-458c-b127-1c8bb8a4c5f1.png)

![image-20211024131056345](https://user-images.githubusercontent.com/93044506/138597022-db307a91-cb24-466d-b789-fc235c9c472f.png)

lakukan setting pada ubuntu_landing html php5.6

````
/etc/nginx/sites-enabled# cd /var/www/html
mkdir lxc_php5.6
cp index.nginx-ubuntu_landing.html lxc_php5.6/index.html
cd lxc_php5.6/
nano index.html
````

![image-20211024131637715](https://user-images.githubusercontent.com/93044506/138597028-6ad9e7f0-0127-4e45-9b93-0bbf7a0e0e59.png)



kemudian melakukan pengecekkan

![image-20211024131644482](https://user-images.githubusercontent.com/93044506/138597031-11a68593-7ef7-40d2-92f6-d5e607dcd9ce.png)
5. LXC ubuntu_landing harus auto start ketika vm dinyalakan, hal ini digunakan untuk menjaga agar website company profile tidak mengalami *downtime*

````markdown
lxc-ls-f
````

![image-20211024131916101](https://user-images.githubusercontent.com/93044506/138597038-486d60b0-8136-4909-b5c6-f8d046b66b12.png)


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

![image-20211024132610527](https://user-images.githubusercontent.com/93044506/138597049-7dc38692-6f62-41e7-9fd1-ca4a0434cd40.png)

!![image-20211024132627110](https://user-images.githubusercontent.com/93044506/138597052-701ea61a-5b12-4573-b034-7f34530e49f0.png)
![image-20211024132656457](https://user-images.githubusercontent.com/93044506/138597062-01856ac7-ba29-401a-863c-a5b8e81700f7.png)

![image-20211024132929489](https://user-images.githubusercontent.com/93044506/138597066-3c5b73f1-5009-4b77-bd7a-bd79dd7efbcb.png)

![image-20211024133139655](https://user-images.githubusercontent.com/93044506/138597070-9697fe51-4dd2-46a6-9e72-11decc402cc5.png)


7. untuk kebutuhan presentasi mereka, browser di laptop mereka harus dapat mengakses ketiga url tersebut

   

![image-20211024133358421](https://user-images.githubusercontent.com/93044506/138597075-361dc839-bbde-449a-aca1-619ce5a6249b.png)

![image-20211024133405035](https://user-images.githubusercontent.com/93044506/138597079-08ecc66f-ed62-423e-80fd-87d92d75333b.png)
![image-20211024133411930](https://user-images.githubusercontent.com/93044506/138597083-e80414ea-2d16-47de-aabc-366815f8c893.png)


8. Menyiapkan analisa untuk diserahkan ke CTO

   a. mengapa untuk kebutuhan php5.6 tidak bisa menggunakan ubuntu 16.04, sehingga perlu diganti os ke debian 9?

   b. kenapa harus menggunakan virtualisasi LXC pada skema website yang akan didevelop

   c. apa yang dimaksud dengan proxy server? kenapa vm.local bisa kita anggap sebagai proxy server?



a. karena pada ubuntu 16.04 php7 adalah sebagai standart untuk menggunakan ubuntunya, dan php5 tidak termasuk dalam standart ubuntu 16.04. jadi ketika ingin menggunakan php5.6 lebih baik mengganti os ke debian 9

b.   Linux Containers adalah metode virtualisasi tingkat sistem operasi untuk menjalankan beberapa sistem Linux terisolasi yang disebut wadah pada satu Host kontrol. Karena LXC menyediakan virtualisasi tingkat sistem operasi, itu bukan melalui mesin virtual yang penuh, tetapi ia menyediakan lingkungan virtualnya sendiri yang memiliki proses dan ruang jaringan sendiri. dan juga karena pemakaian resource server yang jauh lebih ringan

c. proxy server adalah sebuah komputer server atau program komputer yang dapat bertindak sebagai komputer lainnya untuk melakukan request terhadap content dari Internet dapat menyimpan, sementara berupa cache file html server lain untuk mempercepat akses internet

Vm.local bisa dianggap sebagai proxy server karena  sebagai perantara antara client hosts, dengan server yang ingin diaksesnya. 
