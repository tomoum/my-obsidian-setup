---
number headings: auto, first-level 1, max 6, 1.1
---

#linux #ssh 
Sources: 
- [SSH Commands Cheat Sheet for Linux Users | ComputingForGeeks](https://computingforgeeks.com/ssh-cheatsheet-for-sysadmins/)
## 0.1 Setting Up SSH from Windows 10/11
1.  connect to the pi using monitor and keyboard
2.  edit the sshd_config file etc/ssh/sshd_config to enable password authentication
3.  ssh into the pi as normal ssh username@hostname-pi
4.  Create the ssh keys using ssh-keygen -t rsa -f "rpi1_rsa"
5.  add the rpi ip address to routers lan dhcp![[telus-router-lan-dhcp-menu.png]]
6. add the following to the ssh config file:

```config
Host rpi1
HostName 192.168.1.253
User muhab
PubKeyAuthentication yes
IdentityFile ~/.ssh/rpi1_rsa
```
then revert password authentication at the server

## 0.2 SSH via pem file ( private key)

If you want to access a remote server using a Pem key, the command syntax is:

$ ssh -i /path/to/file.pem [\[email protected\]](https://computingforgeeks.com/cdn-cgi/l/email-protection)

A path to private key file follows after **\-i** flag.

## 0.3 Connect to a non-standard  ssh port:

The default SSH port is 22, to access a remote system with a different service port, use the **\-p** option.

$ ssh -p 2222 [\[email protected\]](https://computingforgeeks.com/cdn-cgi/l/email-protection)

Here, we’re connecting to the SSH server running on port 2222. The port has to be allowed on the firewall.

## 0.4 Connect and forward the authentication agent

Use the **\-A** option to enable the forwarding of the authentication agent.

$ ssh -A [\[email protected\]](https://computingforgeeks.com/cdn-cgi/l/email-protection)

This can also be specified on a per-host basis in a configuration file.

## 0.5 Connect and execute a command on a remote server:

At times you want to run a command on bash shell on a remote server. This is achieved by passing the command and its options after the server part.

$ ssh -t [\[email protected\]](https://computingforgeeks.com/cdn-cgi/l/email-protection)'the-remote-command'

**\-t**  is used to force pseudo-terminal allocation. This can be used to execute arbitrary screen-based programs on a remote machine, which can be very useful, e.g. when implementing menu services.

As an example, let’s connect to a server and do a ping to 8.8.8.8, with a count of 3.

$ ssh outboundmx-01 'ping -c 3 8.8.8.8'
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp\_seq=1 ttl=60 time=6.74 ms
64 bytes from 8.8.8.8: icmp\_seq=2 ttl=60 time=7.27 ms
64 bytes from 8.8.8.8: icmp\_seq=3 ttl=60 time=6.77 ms

--- 8.8.8.8 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2004ms
rtt min/avg/max/mdev = 6.740/6.930/7.271/0.241 ms

SSH session will exit after executing specified commands.

## 0.6 Tunnel an X session over SSH:

The **\-X** option in ssh is used to enable X11 forwarding. This can also be specified on a per-host basis in a configuration file. X11 forwarding can be disabled using **\-x** Disables option.

ssh -X [\[email protected\]](https://computingforgeeks.com/cdn-cgi/l/email-protection)

An example below will:

-   Redirect traffic with a tunnel between localhost (port **8080**) and a remote
-   host (remote.example.com:5000) through a proxy (personal.server.com):

$ ssh -f -L 8080:remote.example.com:5000 [\[email protected\]](https://computingforgeeks.com/cdn-cgi/l/email-protection) -N

**\-N**  means do not execute a remote command. This is useful for just forwarding ports.

## 0.7 Launch a specific X application over SSH:

Use the **\-X** option to launch an application through ssh session.

$ ssh -X -t [\[email protected\]](https://computingforgeeks.com/cdn-cgi/l/email-protection) 'firefox'

This will launch Firefox application and display UI on the local machine.

## 0.8 Create a SOCKS proxy tunnel

$ ssh -D 9999 [\[email protected\]](https://computingforgeeks.com/cdn-cgi/l/email-protection)

This will create a SOCKS proxy on localhost and port  **10000**. The way this works is by allocating a socket to listen to port on the local side, optionally bound to the specified bind\_address. Whenever a connection is made to this port, the connection is forwarded over the secure channel, and the application protocol is then used to determine where to connect to from the remote machine.

Currently the SOCKS4 and SOCKS5 protocols are supported, and ssh will act as a SOCKS server. Note that only root can forward privileged ports.

## 0.9 SSH with data compression and encryption

To request compression of all data (including stdin, stdout, stderr, and data for forwarded X11, TCP and UNIX-domain connections, **\-C** option is used. This is desirable when working with modems and other slow connections systems. Do not use this on faster networks since it will just slow things down.

The compression algorithm is the same used by gzip. **\-c** is used to specify the cipher specification for encrypting the session. More than one listing is done by separating them with commas. Example

$ ssh [\[email protected\]](https://computingforgeeks.com/cdn-cgi/l/email-protection) -C -c blowfish -X

**\-X** –> Use an X session  
**\-C** –> Do data compression  
**\-c** –> Use blowfish encryption for ssh session

## 0.10 SSH copy files

An example below shows how to compress files on a remote server and copy to the local system by piping to tar. Compression and uncompression is done using **tar** command. This is useful if you don’t have **scp** or **rsync** which act as ssh clients.

$ ssh  [\[email protected\]](https://computingforgeeks.com/cdn-cgi/l/email-protection) "cd ~/mydir; \\
tar zcf - file1.txt file2.txt" | tar zxf -

confirm if copied

$ ls file1.txt file2.txt

## 0.11 1Force Public key Copy to a remote server

You’re trying to copy ssh key but keeps getting a failure. You can force the copy using the commands:

$ SSH\_OPTS='-F /dev/null' ssh-copy-id  [\[email protected\]](https://computingforgeeks.com/cdn-cgi/l/email-protection)

## 0.12 Save private key passphrase

With ssh, you can configure authentication agent to save password so that you won’t have to re-enter your passphrase every time you use your SSH keys.

eval $(ssh-agent) # Start agent on demand
ssh-add # Add default key
ssh-add -l # List keys
ssh-add ~/.ssh/id\_rsa # Add specific key
ssh-add -t 3600 ~/.ssh/id\_rsa # Add with timeout
ssh-add -D # Drop keys

## 0.13 Mount folder/filesystem through SSH

Install SSHFS from [https://g](https://github.com/libfuse/sshfs)[ithub.com/libfuse/sshfs .](https://github.com/libfuse/sshfs)

Installation and usage of SSHFS are covered on a different article:

[Installing sshfs and using sshfs on Ubuntu / Fedora / Arch](https://computingforgeeks.com/installing-using-sshfs-ubuntu-fedora-arch-centos/)

This command will mount remote directory to the local machine.

$ sshfs [\[email protected\]](https://computingforgeeks.com/cdn-cgi/l/email-protection):/path/to/folder /path/to/mount/point

Once done, you can unmount directory using:

$ fusermount -u mountpoint

## 0.14 Read files using macs through SSH

Documentation is on [Emacs mount Remote files](https://www.gnu.org/software/emacs/manual/html_node/emacs/Remote-Files.html)

After installing Emacs, reading of the remote file is done using:

$ emacs /ssh:[\[email protected\]](https://computingforgeeks.com/cdn-cgi/l/email-protection):/path/to/file

## 0.15 Deleting IP address/hostname on ~/.ssh/known\_hosts file.

Sometimes you want to copy ssh key to a remote server and you get a warning that the IP/hostname already exist in **~/.ssh/known\_hosts**, to remove the entry, use:

$ ssh-keygen -f .ssh/known\_hosts -R  ip-or-hostname

## 0.16 Update SSH Key passphrase

Use our guide for updating or changing an SSH key passphrase.

[How to change or update SSH key Passphrase on Linux / Unix](https://computingforgeeks.com/how-to-change-or-update-ssh-key-passphrase-on-linux-unix/)  

## 0.17 Changing SSH Service Port

The following guide should be helpful.

[Changing SSH Port on CentOS/RHEL 7/8 & Fedora 31/30/29 With SELinux Enforcing](https://computingforgeeks.com/change-ssh-port-centos-rhel-fedora-with-selinux/)

