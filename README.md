# Prepping an Ubuntu Machine to Run an Art Installation 24/7

The following document was tested on an Ubuntu 16.04 LTS install. Your mileage on other distros and versions may vary. This document is inspired by Blair Neal's (@laserpilot) [*Installation up 4evr*](https://github.com/laserpilot/Installation_Up_4evr) guide.

1. Create autolaunch/relaunch scripts
2. Make these scripts run at startup with Startup Applications
3. Install root cronjob to reboot the machine daily
4. Set the time to the correct timezone for installation location
5. Configure autologin in user settings
6. Disable screen lock and snooze
7. Disable automatic software updates
8. Disable notifications with NoNotifications
9. Install TeamViewer for remote monitoring and access
10. Protect your personal privacy

## 1. Autolaunch/relaunch scripts

Its helpful to have scripts that manage the launching and upkeep of all installation related applications and services. For an example of such a `start_installation.sh` BASH script, see [here](https://gist.github.com/brannondorsey/b2f698283880061e4c935180c87aebd7).

**Note:** you can kill that process with Ctrl-c or, if it was launched as a daemon, `kill $(pgrep -f bash start_installation.sh)`.

## 2. Run on boot with Startup Applications

Click the spotlight and search "Startup Applications". Create an `on_startup.sh` script like the one below and add it as a startup item:

```bash
#!/bin/bash

# Apps sun from Startup Applications may not always have its PATH
# set as you would expect. If you need access to certain commands,
# add their paths manually like so.
export PATH=$PATH:/home/art/.nvm/versions/node/v6.11.2/bin

cd /path/to/start/installation/parent/folder
bash start_installation.sh # > /home/$USER/Desktop/startup.log 2>&1 # optionally log to Desktop
```

## 3. Install root cronjob to reboot daily

Open a root crontab with:

```bash
sudo crontab -e
```

This crontab reboots the machine daily at 3AM. To generate your own crontab, you can use http://crontab.guru.

```
# reboot at 3 AM each day
0 3 * * * /sbin/shutdown -r now
```

Save the crontab and exit.

## 4. Set the correct time

System Settings > Time & Date

## 5. Configure autologin in user settings

System Settings > User Accounts > YOUR_USER. Click the unlock icon on the top right and enter your password.

- Automatic Login: On

## 6. Disable screen lock and snooze

System Settings > Brightness & Lock

- Turn screen off when inactive for: Never
- Lock: Off
- Require my password when waking from suspend: Uncheck

System Settings > Power

-  Suspend when inactive for: Don't suspend

System Settings > Security & Privacy > Security

Require my password when:

- Waking from suspend: Uncheck
- Returning from blank screen: Uncheck

## 7. Disable automatic software updates

System Settings > Software & Updates > Updates

- Automatically check for updates: Never
- When there are other updates: Display every two weeks
- Notify me of a new Ubuntu version: Never

## 8. Disable notifications with NoNotifications

```
sudo add-apt-repository ppa:vlijm/nonotifs
sudo apt-get update
sudo apt-get install nonotifs
```

Open NoNotifications using the spotlight search bar. You should see a small green icon appear on the top right menu bar. Click the icon and select "Don't disturb". Next select preferences and check "Run on startup".

## 9. Install TeamViewer for remote monitoring and access

Download and install TeamViewer for Ubuntu/Debian Linux. Add the machine to your contacts list but **do not** stay signed in (otherwise anyone with access to the machine can login to all of your computers). Enable "Easy Access" for your contact.

## 10. Protect your personal privacy

### Browser Login Sessions and History

The exact method for doing this depends on the browser, but usually there is a history tab in the nav bar. Be sure to delete all history from the beginning of time, including:

- Browsing and download history
- Forms and search history
- Cookies
- Cache
- Active Logins

### Ubuntu App Usage

System Settings > Security & Privacy > Files & Applications

- Record file and application usage: Off

On this screen: Clear Usage Data > From all time > OK

Optionally you may also disable sending Canonical telemtry data with:

System Settings > Security & Privacy > Diagnostics

- Send error reports to Canonical: Uncheck
- Send Occasional system information to Canonical
