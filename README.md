Example Ubuntu 24 Server Autoinstall with LVM Config

This install uses virt-install to setup an Ubuntu virtual machine with
LVM, using autoinstall. The host system will need KVM, virt-install, 
libvirt-clients, virt-manager, etc... Script values are hard coded for 
readability. 

Machine will be set to use dhcp- autoinstall should do this automatically for
[en*,eth*] interfaces. Currently split with separate volumes/mounts specified by
CIS benchmarks. 

The ubuntu-create-vm-command file has the command needed to creat the vm. Change
resources, ISO location, and network interface as needed. You must change the 
<webserver_ip:3003> to whatever webserver/port you're hosting the autoinstall files on.

NOTE- 'ip=dhcp' is set as a kernel arg, networking is needed or else the loading of 
autoinstall files will fail. You can statically assign networking info by replacing 
'ip=dhcp' appropriately.


For webserver setup, you can use apache/nginx/etc, or for a temporary testing setup,
you can use python. 

The webserver will need three files:
- user-data
- vendor-data
- meta-data

The vendor-data and meta-data files don't need any content, but may need to be there, if
nothing else, to reduce logged webserver errors:
```
mkdir www
cd www
touch {vendor-data,meta-data}
```

Place the user-data file in the same www directory, and edit contents as needed. The default
credentials are ubuntu/ubuntu. Change sizing of LVM volumes as needed.

When the files are in place, you can run via python with:
```
python3 -m http.server 3003
```

When substituting the webserver ip/port in the 'virt-install' command, make sure to use the 
external IP/port, and that firewalls allow access.


Other Notes- 
- When looking for autoinstall options, check the reference docs, or you can do a manual
install of Ubuntu with the needed options, and look in the /var/log/autoinstall-user-data 
file after complete install to view syntax.


Reference Docs-
Autoinstall config reference:
- https://canonical-subiquity.readthedocs-hosted.com/en/latest/reference/autoinstall-reference.html
Curtin Storage Options:
- https://curtin.readthedocs.io/en/latest/topics/storage.html
