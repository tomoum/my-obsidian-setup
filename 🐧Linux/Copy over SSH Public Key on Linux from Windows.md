#windows #ssh #linux #ubuntu #cli

1. edit the sshd_config file etc/ssh/sshd_config to enable password authentication 
2. ssh into the pi as normal `ssh username@hostname-pi`
3. Use the following command to copy the public key over type 
```bash
type $env:USERPROFILE\.ssh\id_rsa.pub | ssh {IP-ADDRESS-OR-FQDN} "cat >> .ssh/authorized_keys"
```

> [!note]+
> `sshd_config` is for server ssh config i.e used for when someone is trying to ssh into the host/or 
> machine settings file is on
> `ssh_config` is used when trying to ssh into another machine using the current machine 
