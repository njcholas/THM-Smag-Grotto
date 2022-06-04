# THM-Smag-Grotto

simple writeup

------------------------------------------------------

nmap -sC -sV &IP:

Starting Nmap 7.91 ( https://nmap.org/ ) at 2021-09-13 17:31 -03
Nmap scan report for 10.10.25.124
Host is up (0.20s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 74:e0:e1:b4:05:85:6a:15:68:7e:16:da:f2:c7:6b:ee (RSA)
|   256 bd:43:62:b9:a1:86:51:36:f8:c7:df:f9:0f:63:8f:a3 (ECDSA)
|_  256 f9:e7:da:07:8f:10:af:97:0b:32:87:c9:32:d7:1b:76 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Smag
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

------------------------------------------------------

using ffuf to find new directories:

https://user-images.githubusercontent.com/67773431/133356472-bc48fef4-600a-4eeb-8d32-4f6f1ad0c4e1.png
![2](https://user-images.githubusercontent.com/67773431/133356475-e9eaef4d-ffdd-4cbc-8ce5-6c135ceb00fa.png)

------------------------------------------------------

accessing the mail directory:

![3](https://user-images.githubusercontent.com/67773431/133356478-2c1b8a63-4755-43f2-ab9f-1c44d920519d.png)

------------------------------------------------------

donwloading the pcap file:

![4](https://user-images.githubusercontent.com/67773431/133356479-f635cbdd-dbf8-4899-b42e-63275a3fe8a3.png)

------------------------------------------------------

following the tcp stream from the 4th package:

![5](https://user-images.githubusercontent.com/67773431/133356480-050cfd0e-765c-4c5b-92e1-a8f017f53196.png)

------------------------------------------------------

add the new host to /etc/hosts

------------------------------------------------------

acessing the new virtual host:

![6](https://user-images.githubusercontent.com/67773431/133356481-13d4e734-bd4e-408e-931e-aa66ea9b00c9.png)

------------------------------------------------------

accessing the login.php directory u will find a login page, use the credencials that u got analyzing the pcap file

------------------------------------------------------

at the admin.php directory u can send commands to the web server. use this reverse shell and set a netcat listener at your 
host machine:

![7](https://user-images.githubusercontent.com/67773431/133356482-1f71bb9a-fa5c-45d7-8dea-5b435efa36a5.png)
![8](https://user-images.githubusercontent.com/67773431/133356484-7f96070e-0bd4-4510-b7d4-5612097b7d63.png)

------------------------------------------------------

then u get a webshell

![9](https://user-images.githubusercontent.com/67773431/133356485-a0f06850-5996-4514-96fc-d19a9f48fcff.png)

------------------------------------------------------

using linpeas.sh to enumerate the machine u will find this:

![linpeas](https://user-images.githubusercontent.com/67773431/171980841-b965f772-30ed-4c90-94b6-b657c3440582.png)

------------------------------------------------------

the file jake_id_rsa.pub.backup is being sent for the authorized_keys at jake's directory. with that in mind u can generate a ssh key pair at your local machine with the command:

![11](https://user-images.githubusercontent.com/67773431/133356487-850e868f-2a4b-4991-9b69-44ffcfa92565.png)

------------------------------------------------------

now u can send your public ssh key to the file jake_id_rsa.pub.backup

![12](https://user-images.githubusercontent.com/67773431/133356488-c2de5c88-0d2b-4a03-b43f-becacf9f5cd2.png)

------------------------------------------------------

use your private key to access the ssh

![13](https://user-images.githubusercontent.com/67773431/133356489-61421b99-8bfd-4e78-a8ae-082a9220bf19.png)

------------------------------------------------------

cat the user.txt flag

![user](https://user-images.githubusercontent.com/67773431/171981365-4e9a20fa-c09b-41d0-8005-c9b2f551f836.png)

------------------------------------------------------

see jake's permissions using sudo -l:

![15](https://user-images.githubusercontent.com/67773431/133356494-ac7249ea-6e6e-43bb-a8e9-ef0be5664f2b.png)

------------------------------------------------------

jake have NOPASSWD right to run /usr/bin/apt-get (https://vk9-sec.com/apt-get-privilege-escalation/)

![16](https://user-images.githubusercontent.com/67773431/133356495-d8babc19-c666-4c01-b18e-d896d687c419.png)

------------------------------------------------------

![17](https://user-images.githubusercontent.com/67773431/133356496-ffa41dbe-0823-4074-ac5b-4a95f9777686.png)



