---
layout: post
title: Switch from Snap to DEB Firefox on Kubuntu
categories: [Linux]
tags: [Kubuntu, Linux]
fullview: true
---

Steps may change in the future.  

- `sudo snap remove firefox`  

- Follow https://support.mozilla.org/en-US/kb/install-firefox-linux#w_install-firefox-deb-package-for-debian-based-distributions  
  - However, in step 5: onfigure APT to prioritize packages from the Mozilla repository, use the command below:  
  ```
    echo '
    Package: *
    Pin: origin packages.mozilla.org
    Pin-Priority: 1001

    Package: firefox
    Pin: version 1:1snap*
    Pin-Priority: -1
    ' | sudo tee /etc/apt/preferences.d/mozilla-firefox
  ```

- sudo apt install firefox  

- To ensure that unattended upgrades do not reinstall the snap version of Firefox:  
```
echo 'Unattended-Upgrade::Allowed-Origins:: "namespaces/moz-fx-productdelivery-pr-38b5/repositories/mozilla:mozilla";' | sudo tee /etc/apt/apt.conf.d/51unattended-upgrades-firefox
```

