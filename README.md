# test ansible on arm platform

## [offical documents](https://docs.ansible.com/ansible/2.9/index.html)

## [basic concepts](https://docs.ansible.com/ansible/2.9/network/getting_started/basic_concepts.html)

## [nodes request](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

- Control node
  - Python 2 (version 2.7) or Python 3 (versions 3.5 and higher)
  - ansible `sudo apt install ansible`
- Managed node
  - ssh(dropbear is also worked, bust just tested script module and raw module)
  - sftp(**transfers modules** using SFTP. If that’s not available, you can switch to SCP in ansible.cfg)
  - Python 2 (version 2.6 or later) or Python 3 (version 3.5 or later)
    - if python is not available, you may see error message like this `"module_stdout": "/bin/sh: /usr/bin/python: No such file or directory\r\n"`
    - **but** Ansible’s **raw module**(run command on target node), and the **script module**(copy local script to target host and run it. script will transfer to `/$HOME/.ansible/tmp/` folder) , do not depend on a client side install of Python to run.
    - **ansible-playbook may also need python** . ansible-playbook always failed even if only has a raw module task
    - python needs include zlib module

## testing enviroments

testing on qemu

python is not installed in target, so only raw and script modules are available.

## step

- sudo ./run.sh # start qemu
- ifconfig eth0 192.168.100.10

another terminal

- leo@leo-vm:~/work/ansible-cn/playbook$ ansible n1 -m raw -a "touch /tmp/ansible.txt" -i inventory.ini
```
192.168.100.10 | SUCCESS | rc=0 >>
Shared connection to 192.168.100.10 closed.
```
- ansible n1 -m script -a /home/leo/work/ansible-cn/playbook/test_script_module.sh -i inventory.ini #test script module
```
192.168.100.10 | SUCCESS => {
    "changed": true,
    "rc": 0,
    "stderr": "Shared connection to 192.168.100.10 closed.\r\n",
    "stdout": "",
    "stdout_lines": []
}
```

## login account

- root:root
- admin:admin


