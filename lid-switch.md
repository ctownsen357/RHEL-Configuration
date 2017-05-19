### Lid Switch
This disables the sleep switch on a laptop from engaging when the laptop screen is set.  Tested on CentOS 7.x and Atomic Host.  I was running a test clust of ten laptops and wanted to disable the machines from going to sleep when the lid was shut.

```
vi /etc/systemd/logind.conf
# Change HandleLidSwitch to ignore
systemctl restart systemd-logind
# close the lid
```
