# Jarkom-Modul-2-IUP5-2021

Group IUP 5 :

Hilmy Hanif Ibrahim (05111942000005)

Salma Rahma Lailia  (05111942000016)

Khairi Wiryawan     (05111942000023)


# Module 2

## no. 1

EniesLobby will be used as DNS Master, Water7 will be used as DNS Slave, and Skypie will be used as Web Server. There are 2 clients, namely Loguetown and Alabasta. All nodes are connected to the Foosha router, so they can access the internet

### Answer

Run the command iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.40.0.0/16 which is used to connect to external networks on the Foosha router

![alt text](https://github.com/salmalailia/Jarkom-Modul-2-IUP5-2021/blob/a95b52fdbf8c0bcde51daede3c2f565aeae56019/ss/1.0.jpeg)


After that, add the command echo "nameserver 192.168.122.0" to all other nodes to set the DNS IP to /root/.bashrc to run every time the project is started with the command echo nameserver 192.168.122.1 > /etc/resolv.conf

![alt text](https://github.com/salmalailia/Jarkom-Modul-2-IUP5-2021/blob/a95b52fdbf8c0bcde51daede3c2f565aeae56019/ss/1.1.jpeg)

## no. 2

create the main website by accessing franky.yyy.com with the alias www.franky.yyy.com in the kaizoku folder

### Answer

on EniesLobby run command apt-get update and apt-get install bind9 -y to install bind9

![alt text](https://github.com/salmalailia/Jarkom-Modul-2-IUP5-2021/blob/a95b52fdbf8c0bcde51daede3c2f565aeae56019/ss/2.1.jpg)

Then edit /etc/bind/named.conf.local by adding :

bash
     zone "franky.IUP5.com" {
             type masters;
             file "/etc/bind/kaizoku/franky.IUP5.com";
     };

![alt text](https://github.com/salmalailia/Jarkom-Modul-2-IUP5-2021/blob/4d051daf409c831850002b7418f7bb1958173c8d/ss/2.2.jpeg)

then created a kaizoku folder in `/etc/bind`. Then copy /etc/bind/db.local to `/etc/bind/kaizoku/franky.IUP5.com`. Then configure the file to have SOA `franky.IUP5.com.`, NS `franky.IUP5.com.`, A record pointing to `IP Skypie`, and CNAME www at `franky.IUP5.com.`.

![alt text](https://github.com/salmalailia/Jarkom-Modul-2-IUP5-2021/blob/4d051daf409c831850002b7418f7bb1958173c8d/ss/2.3.jpeg)

## no. 3

After that make a subdomain super.franky.yyy.com with alias www.super.franky.yyy.com which ruled by the DNS at EniesLobby to Skypie

### Answer

Edited the /etc/bind/kaizoku/franky.IUP5.com file by adding:

bash
         ns1 IN A 10.40.2.4 ; Skype IP
         super IN NS ns1


![alt text] (https://github.com/salmalailia/Jarkom-Modul-2-IUP5-2021/blob/4d051daf409c831850002b7418f7bb1958173c8d/ss/3.1.jpeg)

Then add the zone to /etc/bind/named.conf.local by adding:

bash
     zone "super.franky.IUP5.com" {
             type masters;
             file "/etc/bind/kaizoku/super.franky.IUP5.com";
     };


![alt text](https://github.com/salmalailia/Jarkom-Modul-2-IUP5-2021/blob/4d051daf409c831850002b7418f7bb1958173c8d/ss/3.2.jpg)

Then copy /etc/bind/db.local to `/etc/bind/kaizoku/super.franky.IUP5.com`. Then configure the file to have SOA `super.franky.IUP5.com.`, NS `super.franky.IUP5.com.`, A record pointing to `IP Skypie`, and CNAME www at `super.franky.IUP5 .com.`.

![alt text](https://github.com/salmalailia/Jarkom-Modul-2-IUP5-2021/blob/4d051daf409c831850002b7418f7bb1958173c8d/ss/3.3.jpeg)

## no. 4

Also create a reverse domain for the main domain

### Answer

First, add the zone to /etc/bind/named.conf.local by adding:

bash
     zone "2.40.10.in-addr.arpa" {
             type masters;
             file "/etc/bind/kaizoku/2.40.10.in-addr.arpa";
     };

![alt text](https://github.com/salmalailia/Jarkom-Modul-2-IUP5-2021/blob/4d051daf409c831850002b7418f7bb1958173c8d/ss/4.1.jpeg)

Then copy /etc/bind/db.local to `/etc/bind/kaizoku/2.40.10.in-addr.arpa`. Then configure the file to have SOA `franky.IUP5.com.`,`2.40.10.in-addr.arpa.` which has NS `franky.IUP5.com.`, and 2 which is the 4th byte of the EniesLobby IP has PTR `franky.IUP5.com.`.

![alt text](https://github.com/salmalailia/Jarkom-Modul-2-IUP5-2021/blob/4d051daf409c831850002b7418f7bb1958173c8d/ss/4.2.jpeg)

## no. 5

In order to still be able to contact Franky if the EniesLobby server is damaged, then make Water7 the DNS Slave for the main domain

### Answer

![alt text](https://github.com/salmalailia/Jarkom-Modul-2-IUP5-2021/blob/4d051daf409c831850002b7418f7bb1958173c8d/ss/5.1.jpeg)

On Water7:
Run the command apt-get update and apt-get install bind9 -y to install bind9

![alt text](https://github.com/salmalailia/Jarkom-Modul-2-IUP5-2021/blob/4d051daf409c831850002b7418f7bb1958173c8d/ss/5.2.jpeg)

Then edit /etc/bind/named.conf.local by adding :

bash
     zone "franky.IUP5.com" {
         slave types;
         masters { 10.40.2.2; };
         file "/var/lib/bind/franky.IUP5.com";
     };

![alt text](https://github.com/salmalailia/Jarkom-Modul-2-IUP5-2021/blob/4d051daf409c831850002b7418f7bb1958173c8d/ss/5.2.jpeg)

At EniesLobby:

First edit the franky.IUP5.com zone in /etc/bind/named.conf.local to:

bash
     zone "franky.IUP5.com" {
             type masters;
             notify yes;
             also-notify { 10.40.2.3; };
             allow-transfer { 10.40.2.3; };
             file "/etc/bind/kaizoku/franky.IUP5.com";
     };

![alt text](https://github.com/salmalailia/Jarkom-Modul-2-IUP5-2021/blob/4d051daf409c831850002b7418f7bb1958173c8d/ss/5.3.jpeg)

## no. 6

After that there is a subdomain mecha.franky.yyy.com with the alias www.mecha.franky.yyy.com which was delegated from EniesLobby to Water7 with the IP going to Skypie in the sunnygo folder.

### Answer

both on Water7 EniesLobby:
edit the file /etc/bind/named.conf.options with the comment section dnssec-validation auto; and add the line `allow-query{any;};`.
![alt text](https://github.com/salmalailia/Jarkom-Modul-2-IUP5-2021/blob/4d051daf409c831850002b7418f7bb1958173c8d/ss/6.1.jpeg)

At EniesLobby:

First, edit the file /etc/bind/kaizoku/franky.IUP5.com by adding:

bash
        ns2 IN A 10.40.2.3 ; IP Water7
        mecha IN NS ns2

![alt text](https://github.com/salmalailia/Jarkom-Modul-2-IUP5-2021/blob/4d051daf409c831850002b7418f7bb1958173c8d/ss/6.2.jpeg)

Make sure there is a line allow-transfer { "IP Water7"; }; in the franky.IUP5.com zone in the /etc/bind/named.conf.local file.

![alt text](https://github.com/salmalailia/Jarkom-Modul-2-IUP5-2021/blob/4d051daf409c831850002b7418f7bb1958173c8d/ss/6.3.jpeg)

On Water7:

add the zone to /etc/bind/named.conf.local by adding:

bash
    zone "mecha.franky.IUP5.com" {
            type masters;
            file "/etc/bind/sunnygo/mecha.franky.IUP5.com";
    };

![alt text](https://github.com/salmalailia/Jarkom-Modul-2-IUP5-2021/blob/4d051daf409c831850002b7418f7bb1958173c8d/ss/6.4.jpeg)

then create a sunnygo folder in `/etc/bind`. Then copy /etc/bind/db.local to `/etc/bind/sunnygo/mecha.franky.IUP5.com`. Then configure the file to have SOA `mecha.franky.IUP5.com.`, NS `mecha.franky.IUP5.com.`, record A pointing to `IP Skypie`, and CNAME www at `mecha.franky.IUP5 .com.`.

![alt text](https://github.com/salmalailia/Jarkom-Modul-2-IUP5-2021/blob/4d051daf409c831850002b7418f7bb1958173c8d/ss/6.5.jpeg)
![alt text](https://github.com/salmalailia/Jarkom-Modul-2-IUP5-2021/blob/4d051daf409c831850002b7418f7bb1958173c8d/ss/6.6.jpeg)

## no. 7

To facilitate communication between Luffy and his colleagues, a subdomain was created through Franky with the name general.mecha.franky.yyy.com with the alias www.general.mecha.franky.yyy.com which points to Skypie

### Answer

On Water7:

Edited the /etc/bind/sunnygo/mecha.franky.IUP5.com file by adding:

bash
         ns1 IN A 10.40.2.4 ; Skype IP
         general IN NS ns1

![alt text](https://github.com/salmalailia/Jarkom-Modul-2-IUP5-2021/blob/4d051daf409c831850002b7418f7bb1958173c8d/ss/7.1.jpeg)

Then add the zone to /etc/bind/named.conf.local by adding:

bash
     zone "general.mecha.franky.IUP5.com" {
             type masters;
             file "/etc/bind/sunnygo/general.mecha.franky.IUP5.com";
     };

![alt text](https://github.com/salmalailia/Jarkom-Modul-2-IUP5-2021/blob/4d051daf409c831850002b7418f7bb1958173c8d/ss/7.2.jpeg)

Then copy /etc/bind/db.local to `/etc/bind/sunnygo/general.mecha.franky.IUP5.com`. Then configure the file to have SOA `general.mecha.franky.IUP5.com.`, NS `general.mecha.franky.IUP5.com.`, record A pointing to `IP Skypie`, and CNAME www at `general .mecha.franky.IUP5.com.`.

![alt text](https://github.com/salmalailia/Jarkom-Modul-2-IUP5-2021/blob/4d051daf409c831850002b7418f7bb1958173c8d/ss/7.3.jpeg)

##OBSTACLE
![1635594167446](https://user-images.githubusercontent.com/73702347/139532693-e117997d-edee-4c5b-a157-9280a78b7498.jpg)
![1635594277307](https://user-images.githubusercontent.com/73702347/139532695-3a2f1f23-0885-4705-bc42-c2b5bd14f4cf.jpg)
![1635594188820](https://user-images.githubusercontent.com/73702347/139532696-396effcb-8770-4e78-9f6d-e1b4d4238a19.jpg)
![1635587248287](https://user-images.githubusercontent.com/73702347/139532697-542e46ad-e05c-4e4a-8ac1-82be28790c23.jpg)
![1635587174496](https://user-images.githubusercontent.com/73702347/139532698-89c40ea8-1d87-461a-af62-f9f019240d2c.jpg)

The problem is that the serial bind is the same as the nameserver. either there are some problems in SOA SOA. Same problem with the resolv yesterday, the Enieslobby IP and Water7 IPs haven't been planted yet.
