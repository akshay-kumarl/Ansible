## ansible inventory file 

vim inventory.ini 
```
ubuntu@54.205.34.182 ansible_ssh_private_key_file=~/virginiakey.pem
```

`ansible -i inventory.ini -m ping all`
