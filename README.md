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


















