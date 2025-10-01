# RootMe - THM 
Goal   | Get root
   --    | --
Description | A ctf for beginners, can you root me?
Score  | 210
Difficulty | easy
## nmap scan
```bash
$ nmap -sV  10.10.233.180
  Nmap scan report for 10.10.233.180
  Host is up (0.11s latency).
  Not shown: 998 closed tcp ports (reset)
  PORT   STATE SERVICE VERSION
  22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
  80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
  Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
###### so, we can see we have to opens port SSH and HTTP

## HTTP
`http://10.10.233.180`
![alt txet](https://github.com/user-attachments/assets/79162dad-117b-4ab2-950d-111f4a36f747)

## Gobuster

Now let's find for hidden directories
```bash
$ gobuster dir -u http://10.10.233.180 -w /usr/share/wordlists/dirb/common.txt
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
/css                  (Status: 301) [Size: 308] [--> http://10.10.233.180/css/]
/index.php            (Status: 200) [Size: 616]
/js                   (Status: 301) [Size: 307] [--> http://10.10.233.180/js/]
/panel                (Status: 301) [Size: 310] [--> http://10.10.233.180/panel/]
/server-status        (Status: 403) [Size: 276]
/uploads              (Status: 301) [Size: 312] [--> http://10.10.233.180/uploads/]
```
Cool. we have two hidden directories, first let's check for /panel directory
