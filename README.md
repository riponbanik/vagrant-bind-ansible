# vagrant-bind-ansible
Build Bind Name Server (DNS) Master & Slave using Vagrant and Ansible Provisioner

The repository is inspired by [vagrant-bind](https://github.com/jonatasbaldin/vagrant-bind)
Removed the client and add customization regarding NM_CONTROL and DNSSEC

## Build the server
```
git clone https://github.com/riponbanik/vagrant-bind-ansible
cd vagrant-bind-ansible
vagrant up 
```

## When you're done, you can shut down the server using
```
vagrant halt
```

## Cleanup
```
vagrant destroy -f
```


