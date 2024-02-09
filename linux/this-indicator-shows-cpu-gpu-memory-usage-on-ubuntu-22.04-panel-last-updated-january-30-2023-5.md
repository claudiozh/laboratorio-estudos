---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 💻 This Indicator Shows CPU, GPU, Memory Usage on Ubuntu 22.04 Panel Last updated: January 30, 2023 — 5

There are several Gnome Shell extensions to display system resource usage in Ubuntu, but in this tutorial I’m going to introduce an indicator that works in not only _GNOME_, but also _Unity_, _MATE_, and _Budgie_ desktop environments.

It’s [Indicator-SysMonitor](https://github.com/fossfreedom/indicator-sysmonitor), a free and open-source applet developed by the leader of Ubuntu Budgie team.

With it, user can display the usage and/or temperature of the following system resource in top-panel:

* average CPU usage.
* NVIDIA GPU utilization.
* Memory usage.
* network upload/download speed.
* CPU, NVIDIA GPU temperature.
* Swap usage.
* Public IP address.

[<img src="https://ubuntuhandbook.org/wp-content/uploads/2023/01/indicator-sysmonitor.webp" alt="" data-size="original">](https://ubuntuhandbook.org/wp-content/uploads/2023/01/indicator-sysmonitor.webp)

Most important is that user can customize the output, by setting which one or ones to display, in which order with which text. User just need to click the indicator on panel to open ‘Preferences’ dialog from pop-down menu, and format the output code in ‘Advanced’ tab.

[![](https://ubuntuhandbook.org/wp-content/uploads/2023/01/edit-indi-sysmonitor.webp)](https://ubuntuhandbook.org/wp-content/uploads/2023/01/edit-indi-sysmonitor.webp)

### How to Install Indicator-Sysmonitor

The developer has an [Ubuntu PPA](https://launchpad.net/\~fossfreedom/+archive/ubuntu/indicator-sysmonitor) contains the packages for Ubuntu 18.04, Ubuntu 20.04, Ubuntu 22.04, Ubuntu 22.10, and even the next Ubuntu 23.04.

1\. First, press Ctrl+Alt+T on keyboard to open a terminal window. When it opens, run command to add the PPA:

```
sudo add-apt-repository ppa:fossfreedom/indicator-sysmonitor
```

_Type user password when it asks and hit Enter to continue._

[![](https://ubuntuhandbook.org/wp-content/uploads/2023/01/indicator-sysmonitor-ppa-600x210.webp)](https://ubuntuhandbook.org/wp-content/uploads/2023/01/indicator-sysmonitor-ppa.webp)

2\. For the old Ubuntu 18.04, you need to manually refresh package index after adding PPA:

```
sudo apt update
```

3\. And, install the indicator applet via command:

```
sudo apt install indicator-sysmonitor
```

[![](https://ubuntuhandbook.org/wp-content/uploads/2023/01/apt-indicator-sysmonitor-600x217.webp)](https://ubuntuhandbook.org/wp-content/uploads/2023/01/apt-indicator-sysmonitor.webp)

Finally, search for and open the applet like a normal application (it has same icon to System Monitor).

[![](https://ubuntuhandbook.org/wp-content/uploads/2023/01/open-indicator-sysmonitor.webp)](https://ubuntuhandbook.org/wp-content/uploads/2023/01/open-indicator-sysmonitor.webp)

And click on the applet to open _Preferences_, and turn on start at login, configure output layout, refresh interval, etc.

### Uninstall Indicator-Sysmonitor

You can close the applet by clicking on it in panel and select “Quit”. And remove the package at any time by running a single command in terminal window:

```
sudo apt remove indicator-sysmonitor
```

Also remove the PPA repository, either by running the command below or open “Software & Updates”and remove source line under “Other Software” tab.

```
sudo add-apt-repository --remove ppa:fossfreedom/indicator-sysmonitor
```
