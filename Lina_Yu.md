# Lina_Yu - [THM](https://tryhackme.com/room/lianyu)

## 1.Nmap

 I began by scanning the target machine withe nmap to finde open ports.

 ``` bash
$ nmp 10.10.106.144 -sV
PORT    STATE SERVICE VERSION
21/tcp  open  ftp     vsftpd 3.0.2
22/tcp  open  ssh     OpenSSH 6.7p1 Debian 5+deb8u8 (protocol 2.0)
80/tcp  open  http    Apache httpd
111/tcp open  rpcbind 2-4 (RPC #100000)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

The scan showed several open ports, including web server

## 2.Web Enummeration

#### I started by checking the website on port 80. The main page didn't have any useful information.
![alt](https://github.com/user-attachments/assets/b8ffd4d4-287c-4427-b106-8a31056bdc25)

#### Next, i used gobuster to search for hidden directories
#### Findings:
![alt](https://github.com/user-attachments/assets/226a7ff9-25d7-43a5-8215-cb78394331b0)

### The /island Directory

#### Visiting the /island page didn't show anything obvious.
![alt](https://github.com/user-attachments/assets/941d868f-9634-41d1-840c-2906e90827d8)


#### I checked the page source code and found a hidden secret code
![alt](https://github.com/user-attachments/assets/306a1b57-663e-44f3-ae2d-4c62a472971e)

##### I ran gobuster again on the /island directory to find subdirectories.
#### I found a new directory:
![alt](https://github.com/user-attachments/assets/0b79358f-2337-4155-a872-401a3af967fd)

### The /2100 Directory
#### The /2100 page had a video that wouldn't play. The source code had a hidden comment mentioning .ticket could be file 
![alt](https://github.com/user-attachments/assets/6ae5e122-f69e-437b-806c-844399be003f)

#### I scanned for files with .ticket extension in /2100
![alt](https://github.com/user-attachments/assets/a723878f-abb3-4a93-a8a1-d2812f08b74f)
##### Discovered green_arrow.ticket file containig a password
![alt](https://github.com/user-attachments/assets/f939adac-a850-448e-a0f1-516eb7b3bd79)

## 3.Password Decoding
#### The password from green_arrow.ticket didn't work for FTP. I used Cyberchef and found it was Base58 encoded. Decoding revealed the actule password.
![alt](https://github.com/user-attachments/assets/ae8d4195-8a0a-4969-92bd-31924ec8da67)

## FTP Exploration

#### I accessed FTP using the decoded password with username vigilante and found three images.
![alt](https://github.com/user-attachments/assets/5dc3c0f1-6861-4507-8471-ce4bfde03b4a)

#### I also discovered another user during FTP exploration
![alt](https://github.com/user-attachments/assets/4c52ac51-12b0-4f02-93e3-787736eef617)

## 5.Steganography Analysis

#### The images cotained hidden data requiring passphrase

### Method 1
#### The leave_me_alone.png file had a corrupted signature. i researched online and fixed the PNG signature to maket readable, and i found the passphares
![alt](https://github.com/user-attachments/assets/e596f5e1-df6f-481d-977f-b99380b429de)
![alt](https://github.com/user-attachments/assets/8f277cad-5f86-48ea-8f3a-25834fd0a6bd)
## Method 2

#### Used stegseek with rockyou.txt wordlist 
![alt](https://github.com/user-attachments/assets/917b3bb3-2648-446a-8b27-58afe752fad3)














