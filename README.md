# rssh
route ssh through a series of hosts i.e. multi-hop (for if you / the servers are behind firewalls etc) 

This is a script to enable you to route ssh though a series of host to connect to your final host, and then either run as standard SSH, have a forward proxy, or run a command

```
  rssh ver. 0.3
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

### Example 1 : Traversing a single machine

If you want to connect

* to a remote host called ``destination`` (as user ``chuck``, on default port)
* via a firewall (as user ``jane``, on non-default port 1727)

The corresponding command using rssh is :

    $ rssh jane@firewall:1727 chuck@destination

If you have the firewall and destination defined on your ssh config then it would simply be 

    $ rssh firewall destination


## Example 2 : traversing through two machines

If you want to connect

* to a remote host called ``destination`` (as user ``chuck``, on default port)
* via a firewall (as user ``jane``, on non-default port 1727)
* then via a router  (as user ``bill``, on default port)

Type the following command using rssh :

    $ rssh jane@firewall:1727 bill@router chuck@destination

Again, if you have the firewall, router and destination defined on your ssh config then it would simply be 

    $ rssh firewall router destination


### Example 3 : Traversing through a single machine and setting up a socks proxy in the final destination server

If you want to connect

* to a remote host called ``destination`` (as user ``chuck``, on default port) and have a Socks proxy on port 8889
* via a firewall (as user ``jane``, on non-default port 1727)

The corresponding command using rssh is :

    $ rssh jane@firewall:1727 chuck@destination -8889

If you have the firewall and destination defined on your ssh config then it would simply be 

    $ rssh firewall destination -s 8889


### Using 'common' settings from bashrc on the destination server. 

If you add your bashsettings into a file at ~/.sshrc then these will be used on the destination server (temporarily whist you are logged on). So if you want to include your favourite aliases, functions etc into ~/.sshrc then they will be available to you when logged into the destination server
( incorporating the code from https://github.com/Russell91/sshrc ) 
