---
layout: post
title: Server hosting tips
categories: [Website]
tags: [Website, Linux]
fullview: true
---

1. Add non-root user.
  i. `# useradd USERNAME`  
  ii. `# passwd USERNAME`  
  iii. Add your user to the `wheel` or `sudo` group:  
    - DEB based distros: `# usermod -aG sudo USERNAME`
    - RPM based distros: `# usermod -aG wheel USERNAME`

2. Disable root logins.
  i. `# vim /etc/ssh/sshd_config`  
  ii. Set `PermitRootLogin=no`

3. Use non-root user

4. (Just my preference) Install `zsh` & `oh-my-zsh`

5. Set up SSH keys
  - If no exsisting key, generate one first
  - On local machine: `$ ssh-copy-id USERNAME@IP_ADDRESS`

6. Configure `ufw` firewall

7. Install `Fail2Ban`

8. Use Docker rootless