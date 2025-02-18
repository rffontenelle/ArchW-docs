---
title: "How to Setup"
has_children: true
---
# How to Set Up ArchWSL

## Requirements

* Windows 10 1709 Fall Creators Update 64bit or later.
* Windows Subsystem for Linux feature is enabled.

## Installation Instructions

There are two ways to install ArchWSL.

### Method 1: zip file

1. [Download](https://github.com/yuk7/ArchWSL/releases/latest) the installer zip.
2. Extract all files in zip file to the same directory.
   Please extract to a folder that you have write permission.
   For example, `C:\Program Files` cannot be used since the rootfs cannot be modified there.
3. Run `Arch.exe` to extract the rootfs and register to WSL

As a side note, the executable name is what is used as the WSL instance name.
If you rename it, you can have multiple installs.

### Method 2: appx package

1. [Download the `.appx` and `.cer`](https://github.com/yuk7/ArchWSL/releases/latest)
2. Install `.cer` to the "Trusted Root Certificate Store" of the local machine.
   For details, please refer to the [Install Certificate page](Install-Certificate.md).
   You will need administrator privileges to install the certificate.
3. Install the `.appx`

## Setup after install
### [If you are a WSL1 user, you **must** change the glibc package. Please see Known issues.](Known-issues.md#wsl1--wsl2)

### Setting the root password

```shell
>Arch.exe
[root@PC-NAME]# passwd
```

### Set up the default user

Please see ArchWiki
[Sudo](https://wiki.archlinux.org/index.php/Sudo#Example_entries)
and
[User and groups](https://wiki.archlinux.org/index.php/Users_and_groups) pages.

```shell
>Arch.exe
[root@PC-NAME]# echo "%wheel ALL=(ALL) ALL" > /etc/sudoers.d/wheel
(setup sudoers file.)

[root@PC-NAME]# useradd -m -G wheel -s /bin/bash {username}
(add user)

[root@PC-NAME]# passwd {username}
(set default user password)

[root@PC-NAME]# exit

>Arch.exe config --default-user {username}
    (setting to default user)
```

If the default user has not been changed
([issue #7](https://github.com/yuk7/ArchWSL/issues/7)),
please reboot the computer or alternatively, restart the LxssManager in an Admin
command prompt.

To restart the `LxssManager`, run this:

```batch
net stop lxssmanager && net start lxssmanager
```

### Initialize keyring

Please excute these commands to initialize the keyring.
(This step is necessary to use pacman.)

```shell
>Arch.exe
[user@PC-NAME]$ sudo pacman-key --init

[user@PC-NAME]$ sudo pacman-key --populate

[user@PC-NAME]$ sudo pacman -Syy archlinux-keyring
```

### Install patched glibc (need in WSL1)
Arch's glibc is built for Linux kernel 4.4 and above and does not work with WSL1.

WSL1 users **should** always follow the steps in [Known issues](Known-issues.md#wsl1--wsl2).

### Install systemctl alternative (Optional)
WSL does not have support for systemd however, there are several solutions.
Please see [Known issues](Known-issues.md#systemdsystemctl).
