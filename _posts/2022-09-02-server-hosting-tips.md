---
layout: post
title: Server hosting tips
categories: [Website]
tags: [Website, Linux]
fullview: true
---

1. Add non-root user.  
  I. `# useradd -m USERNAME`  
  II. `# passwd USERNAME`  
  III. Add your user to the `wheel` or `sudo` group:
    - DEB based distros: `# usermod -aG sudo USERNAME`
    - RPM based distros: `# usermod -aG wheel USERNAME`

2. Disable root logins.  
  I. `# vim /etc/ssh/sshd_config`  
  II. Set `PermitRootLogin=no`

3. Change SSH port

4. Use non-root user

5. (Just my preference) Install `zsh` & `oh-my-zsh`

6. Set up SSH keys
  - If no exsisting key, generate one first
  - On local machine: `$ ssh-copy-id USERNAME@IP_ADDRESS` (If SSH port changed: `$ ssh-copy-id -p PORT USERNAME@IP_ADDRESS`)

7. Configure `ufw` firewall

8. Install `Fail2Ban`

9. (If possible) Use Docker rootless

10. (From: https://docs.docker.com/engine/install/linux-postinstall/) If using regular Docker, don't forget:
  - `$ sudo groupadd docker`
  - `$ sudo usermod -aG docker $USER`
  - Reboot

11. (Optional) Disable SMTP port