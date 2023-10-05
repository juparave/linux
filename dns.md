# Setting permanent DNS in Ubuntu/Debian

ref: https://www.tecmint.com/set-permanent-dns-nameservers-in-ubuntu-debian/

## Installing resolvconf in Ubuntu and Debian

    # apt update
    # apt install resolvconf

Check if service is running

    # systemctl status resolvconf.service

If not started, start it and enable as follows

    # systemctl start resolvconf.service
    # systemctl enable resolvconf.service
    # systemctl status resolvconf.service

## Set Permanent DNS Nameservers in Ubuntu and Debian

Open the configuration file

    # vim /etc/resolvconf/resolv.conf.d/head

Add google's DNS's

    nameserver 8.8.8.8
    nameserver 8.8.4.4

Save changes and restart

    # systemctl restart resolvconf.service
    # systemctl restart systemd-resolved.service

Now when you check the /etc/resolv.conf file, the new name server entries
should be stored there

