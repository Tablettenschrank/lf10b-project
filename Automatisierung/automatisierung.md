# Automatisierung

Automatisiert wurden die Security updats von Deb13 Trixie, damit man immer auf dem neusten stand ist.

Generelle updates werden nicht automatisch gemacht, da dies risiken birkt, wie Probleme in Packages das docker nicht mehr richtig funktioniert.



1. **Install the automation tool and the changelog viewer.**

```shellscript
sudo apt update
sudo apt install unattended-upgrades apt-listchanges -y
```





1. **Enable the Daily Schedule**

Configure the system to update package lists and perform upgrades every day (value `"1"`).

**Create/Edit**: `/etc/apt/apt.conf.d/20auto-upgrades`

```javascript
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Unattended-Upgrade "1";
```



1. **Configure Update Origins**

Define which repositories are allowed to trigger an automatic update.

**Edit**: `/etc/apt/apt.conf.d/50unattended-upgrades`

Locate `Unattended-Upgrade::Origins-Pattern` and ensure strictly the **Security** lines are active:

```javascript
Unattended-Upgrade::Origins-Pattern {
//...
    // Only install from the dedicated security repository
    "origin=Debian,codename=${distro_codename}-security,label=Debian-Security";
    "origin=Debian,codename=${distro_codename},label=Debian-Security";
    
    // Comment out normal updates to avoid non-security changes:
    // "origin=Debian,codename=${distro_codename},label=Debian";
//...
};
...
```



1. **Verification**

Perform a dry-run to ensure the configuration is valid and to see which packages would be upgraded.

```shellscript
sudo unattended-upgrades --dry-run --debug
```



1. **Logs & Monitoring**

* **Execution Logs:**`/var/log/unattended-upgrades/unattended-upgrades.log`
* **Systemd Timer Status:**`systemctl status apt-daily-upgrade.timer`

