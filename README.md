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

  
### Hackthebox Redeemer exercise - Redis-cli

First use nmap to scan the ip address of target machine

`nmap -sV -sC 10.129.207.131`

adding -p-, allows for nmap to observe for ports. This takes a very long time, fyi

`nmap -p- -sV 0.129.207.131`

use redis-cli command to interact with redis database 

`redis-cli -h 10.129.207.131`

```bash
10.129.207.131:6379> info
# Server
redis_version:5.0.7
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:66bd629f924ac924
redis_mode:standalone
os:Linux 5.4.0-77-generic x86_64
arch_bits:64
multiplexing_api:epoll
atomicvar_api:atomic-builtin
gcc_version:9.3.0
process_id:756
run_id:905c4091d884f1e7b4984b8e8ecf5277485a30be
tcp_port:6379
uptime_in_seconds:10048
uptime_in_days:0
hz:10
configured_hz:10
lru_clock:8983025
executable:/usr/bin/redis-server
config_file:/etc/redis/redis.conf

# Clients
connected_clients:2
client_recent_max_input_buffer:2
client_recent_max_output_buffer:0
blocked_clients:0

# Memory
used_memory:880544
used_memory_human:859.91K
used_memory_rss:6291456
used_memory_rss_human:6.00M
used_memory_peak:880544
used_memory_peak_human:859.91K
used_memory_peak_perc:100.12%
used_memory_overhead:863064
used_memory_startup:796224
used_memory_dataset:17480
used_memory_dataset_perc:20.73%
allocator_allocated:1476304
allocator_active:1892352
allocator_resident:9138176
total_system_memory:2084024320
total_system_memory_human:1.94G
used_memory_lua:41984
used_memory_lua_human:41.00K
used_memory_scripts:0
used_memory_scripts_human:0B
number_of_cached_scripts:0
maxmemory:0
maxmemory_human:0B
maxmemory_policy:noeviction
allocator_frag_ratio:1.28
allocator_frag_bytes:416048
allocator_rss_ratio:4.83
allocator_rss_bytes:7245824
rss_overhead_ratio:0.69
rss_overhead_bytes:-2846720
mem_fragmentation_ratio:7.50
mem_fragmentation_bytes:5452920
mem_not_counted_for_evict:0
mem_replication_backlog:0
mem_clients_slaves:0
mem_clients_normal:66616
mem_aof_buffer:0
mem_allocator:jemalloc-5.2.1
active_defrag_running:0
lazyfree_pending_objects:0

# Persistence
loading:0
rdb_changes_since_last_save:0
rdb_bgsave_in_progress:0
rdb_last_save_time:1669918262
rdb_last_bgsave_status:ok
rdb_last_bgsave_time_sec:0
rdb_current_bgsave_time_sec:-1
rdb_last_cow_size:417792
aof_enabled:0
aof_rewrite_in_progress:0
aof_rewrite_scheduled:0
aof_last_rewrite_time_sec:-1
aof_current_rewrite_time_sec:-1
aof_last_bgrewrite_status:ok
aof_last_write_status:ok
aof_last_cow_size:0

# Stats
total_connections_received:7
total_commands_processed:9
instantaneous_ops_per_sec:0
total_net_input_bytes:364
total_net_output_bytes:26425
instantaneous_input_kbps:0.00
instantaneous_output_kbps:0.00
rejected_connections:0
sync_full:0
sync_partial_ok:0
sync_partial_err:0
expired_keys:0
expired_stale_perc:0.00
expired_time_cap_reached_count:0
evicted_keys:0
keyspace_hits:0
keyspace_misses:0
pubsub_channels:0
pubsub_patterns:0
latest_fork_usec:718
migrate_cached_sockets:0
slave_expires_tracked_keys:0
active_defrag_hits:0
active_defrag_misses:0
active_defrag_key_hits:0
active_defrag_key_misses:0

# Replication
role:master
connected_slaves:0
master_replid:fc69af3c21816c1193ae099ba80afae97bb0bdc5
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0

# CPU
used_cpu_sys:9.504078
used_cpu_user:9.903295
used_cpu_sys_children:0.001975
used_cpu_user_children:0.000000

# Cluster
cluster_enabled:0

# Keyspace
db0:keys=4,expires=0,avg_ttl=0
```
now that you're in, select database 0 as the instructions ask on hackthebox.

Then, list all the keys. 

Lastly, get the flag!

```bash
10.129.207.131:6379> select 0
OK
10.129.207.131:6379> keys *
1) "flag"
2) "stor"
3) "numb"
4) "temp"
10.129.207.131:6379> get flag
"03e1d2b376c37ab3f5319922053953eb"
```
