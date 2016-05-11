#Regain Root access on Kerlink

You need the uniqe ***usbkey.txt*** file for *your* kerlink.


***This method was done with the Kerlink USB Serial debugger, however it should be possible to do without - see the end of this page for an explaination***

When updating the software, the file "poly-pkt-fwd.sh" is set to run, so let's use this!

##In short steps: 

* Modify the **mnt/fuser-1/thethingsnetwork/poly-pkt-fwd.sh** in dota_thethingsnetwork_v1.3_EU.tar, by appending these lines:

```
echo "root" | /usr/bin/passwd root --stdin
echo -e "root\nroot" | passwd root
echo -e "root\nroot" | (passwd --stdin root)
echo "root:root" | chpassw
```

I'm quite sure which version actually workes on the platform, but one of them does 

* Load Files on to USB (dota_thethingsnetwork_v1.3_EU.tar,produsb.sh,usbkey.txt)

* Insert USB into Kerlink, and wait 10 minutes - you can open a serial connection and see whats going on. (Serial running @ 57600 baud)

* You should be presented with a login prompt, use "root" as username and "root" as password

* You're in!

* Now modify mnt/fuser-1/thethingsnetwork/poly-pkt-fwd.sh and comment/delete the lines we appended. Otherwise the password will be changed everytime the script is run.

* Finally you can run "dropbear" to enable SSH access, if you want ssh to start after a reboot, edit the file ***/etc/rc.d/rc.local*** and append the line

`dropbear`

##No Serial Debugger
If you don't have access to a serial debugger you should be able to add something like

`echo "dropbear" >>  /etc/rc.d/rc.local`

To the *poly-pkt-fwd.sh* script.

This should enable dropbear on boot. 

** Please note that this is not tested, and will probably add the line many times to **/etc/rc.d/rc.local**