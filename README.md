# Some Tips

```push
sudo ansible-pull -U https://github.com/$USER/ansible_project.git
```

## Install on one host some module with sudo example **-b**

```sudo
ansible -i hosts oldmac -m xbps -a "name=cmatrix state=present" -b
```

Argument **-a**

## Ask password and install on one host some module with sudo example **-k**

```sudo
ansible -i hosts oldmac -m xbps -a "name=cmatrix state=present" -b -k
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

### **Useful facts**

```facts
# Nitro
"ansible_facts": {
  "ansible_cmdline": {
  "BOOT_IMAGE": "/@/boot/vmlinuz-linux-lts",
  "acpi": "force",
  "acpi_backlight": "video",
  "apci_osi": "Linux",
  "console": "tty2",
  "cryptomgr.notests": true,
  "gpt": true,
  "init_on_alloc": "0",
  "initcall_debug": true,
  "intel_idle.max_cstate": "1",
  "intel_iommu": "on,igfx_off",
  "intel_pstate": "active",
  "loglevel": "2",
  "mitigations": "off",
  "module.sig_unenforce": true,
  "msr.allow_writes": "on",
  "net.ifnames": "0",
  "no_timer_check": true,
  "noreplace-smp": true,
  "nowatchdog": true,
  "nvidia-drm.modeset": "1",
  "page_alloc.shuffle": "1",
  "pcie_aspm": "force",
  "quiet": true,
  "rcupdate.rcu_expedited": "1",
  "root": "UUID=eb665389-095f-4904-b7a4-99f42798e327",
  "rootflags": "subvol=@",
  "rw": true,
  "tsc": "reliable",
  "udev.log_level": "0",
  "zswap.compressor": "zstd",
  "zswap.enabled": "1",
  "zswap.max_pool_percent": "10",
  "zswap.zpool": "zsmalloc"
}

# Dietpi
"ansible_cmdline": {
  "BOOT_IMAGE": "/@/boot/vmlinuz-linux-lts",
  "acpi": "force",
  "acpi_backlight": "video",
  "apci_osi": "Linux",
  "console": "tty2",
  "cryptomgr.notests": true,
  "gpt": true,
  "init_on_alloc": "0",
  "initcall_debug": true,
  "intel_idle.max_cstate": "1",
  "intel_iommu": "on,igfx_off",
  "intel_pstate": "active",
  "loglevel": "2",
  "mitigations": "off",
  "module.sig_unenforce": true,
  "msr.allow_writes": "on",
  "net.ifnames": "0",
  "no_timer_check": true,
  "noreplace-smp": true,
  "nowatchdog": true,
  "nvidia-drm.modeset": "1",
  "page_alloc.shuffle": "1",
  "pcie_aspm": "force",
  "quiet": true,
  "rcupdate.rcu_expedited": "1",
  "root": "UUID=eb665389-095f-4904-b7a4-99f42798e327",
  "rootflags": "subvol=@",
  "rw": true,
  "tsc": "reliable",
  "udev.log_level": "0",
  "zswap.compressor": "zstd",
  "zswap.enabled": "1",
  "zswap.max_pool_percent": "10",
  "zswap.zpool": "zsmalloc"
},
{
  "block_available": 25847678,
  "block_size": 32768,
  "block_total": 60025086,
  "block_used": 34177408,
  "device": "192.168.1.207:/mnt/HD/HD_a2",
  "fstype": "nfs",
  "inode_available": 121951686,
  "inode_total": 121970688,
  "inode_used": 19002,
  "mount": "/mnt/Data",
  "options": "rw,relatime,vers=3,rsize=32768,wsize=32768,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,mountaddr=192.168.1.207,mountvers=3,mountport=51466,mountproto=udp,local_lock=none,addr=192.168.1.207",
  "size_available": 846976712704,
  "size_total": 1966902018048,
  "uuid": "N/A"
},

"block_available": 8429827,
  "block_size": 32768,
  "block_total": 11953830,
  "block_used": 3524003,
  "device": "192.168.1.207:/mnt/HD/HD_b2",
  "fstype": "nfs",
  "inode_available": 24284319,
  "inode_total": 24289280,
  "inode_used": 4961,
  "mount": "/mnt/DataRoot",
  "options": "rw,relatime,vers=3,rsize=32768,wsize=32768,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,mountaddr=192.168.1.207,mountvers=3,mountport=51466,mountproto=udp,local_lock=none,addr=192.168.1.207",
  "size_available": 276228571136,
  "size_total": 391703101440,
  "uuid": "N/A"
}

```chroot
ansible -m setup localhost -a 'filter=ansible_is_chroot'
```

```env
ansible localhost -m setup -a 'filter=ansible_env'
```

```device
ansible localhost -m gather_facts -a 'filter=ansible_form_factor'
```

```Symanager
# Systemd | RUNIT | ETC
ansible localhost -m gather_facts -a 'filter=ansible_service_mgr'
```
