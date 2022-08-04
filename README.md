# Some Tips

```push
sudo ansible-pull -U https://github.com/$USER/ansible_project.git
```

```ssh
ansible all --key-file ~/.ssh/ansible -i inventory -m ping
--------------------------

# -m (modules)

ansible all --key-file ~/.ssh/ansible -i hosts -m ping
--------------------------

ansible all -m ping
```

## Run command as **nobody**

```command
- name: Run a command as nobody
  command: somecommand
  become: yes
  become_method: su
  become_user: nobody
  become_flags: '-s /bin/sh'
```

```list
ansible all --list-hosts
ansible all -m gather_facts
ansible all -m gather_facts --limit localhost
```

```update_module
(update)
ansible all -m pacman -a update_cache=true --become --ask-become-pass
ansible all -m apt -a update_cache=true --become --ask-become-pass
ansible all -m xbps -a update_cache=true --become --ask-become-pass

(install package)
ansible all -m pacman -a name=peazip-qt5 --become --ask-become-pass

(update package)
ansible all -m pacman -a "name=peazip-qt5 state="latest" --become --ask-become-pass

(remove the package)
ansible all -m pacman -a "name=peazip-qt5 state="absent" --become --ask-become-pass
```

## Playbooks

```example
nvim install_base.yml
ansible-playbook --ask-become-pass install_base.yml
```

```tags
ansible-playbook --list-tags site.yml
```

### **Play a Single tag**

```tags
ansible-playbook --tags archlinux --ask-become-pass site.yml
ansible-playbook --tags void_install --ask-become-pass site.yml
```

### **Play multiple tags**

```tags
ansible-playbook --tags "parted,base" --ask-become-pass site.yml
```
