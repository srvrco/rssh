# rssh
route ssh through a series of hosts  (for if you / the servers are behind firewalls etc) 

This is a script to enable you to route ssh though a series of host to connect to your final host, and then either run as standard SSH, have a forward proxy, or run a command

```
  rssh ver. 0.1
  route ssh through a series of hosts

  Usage: rssh [-h|--help] [-v] [-s socks_port] sever1 server2 [server3] [server4 ....etc] [-c command]

  Options:
  -h, --help  Display this help message and exit.
  -v          verbose output from ssh
  -d          debug on
  -D          debug off
  -s nnnn     socks port
  -c command  command to run on remote server

  note: This script assumes that any hosts in your ~/.ssh/config file have a non-indented Host
        line and the rest of the items related to that host are indented.
```

The script reads any hosts you have in your standard ~/.ssh/config file and uses the main attributes for those hosts.   It assumes that your config file is indented in a convetional manner i.e. 
```
Host server1
 Hostname 8.8.8.8
 PasswordAuthentication yes

Host myhomeserver
 Hostname homeserver.mydomain.com
 KexAlgorithms curve25519-sha256@libssh.org,diffie-hellman-group-exchange-sha256,diffie-hellman-group14-sha1

Host devel
 Hostname dedicated862.example.com
 Port 1292
```

The script works by creating a temporary ssh config file, with all the parameters needed, then running ssh with that.   This enables you to then easily create a socks proxy or whatever you need 
