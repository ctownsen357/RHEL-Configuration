# rhel-tmp
How to configure RHEL 7.x to persist fstab changes to /tmp allocation after a reboot.

On a project where I needed to allocate /tmp to a separate physical drive, I initially thought all I needed to do was change `/etc/fstab`.  After testing with a reboot, everything went back to the default setup and my `/etc/fstab` changes were ignored.

I discovered that on cloud based RHEL 7.x one needs to enable `tmp.mount`.  `tmp.mount` is the systemd based init system for configuring how /tmp is handled.

My `/etc/fstab` addition:
```
/dev/<device_name>   /tmp     ext4    rw,nosuid,nodev 0   2
```

After the `/etc/fstab` change the following commands persist the changes:
```
sudo mount -a
sudo systemctl unmask tmp.mount
sudo systemctl enable tmp.mount
sudo systemctl start tmp.mount
systemctl status tmp.mount # should show /tmp mapped to the /dev/<device_name>
sudo reboot # df -h and systemctl status tmp.mount should both show /tmp mapped to the device
```
