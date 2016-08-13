# LCDproc for pfSense #

This fork of [pfsense/FreeBSD-ports](https://github.com/pfsense/FreeBSD-ports) is where I'm updating the LCDproc package to be compatible with pfSense 2.3 onward.

The ports package [has been created](/Treer/FreeBSD-ports/releases), remaining tasks:
* Add realtime traffic features to LCDproc
* Update UI to use Bootstrap

## Installation ##

You can type these commands into the web GUI, via Diagnostics -> Command Prompt -> Execute Shell Command

Install the freetype2 library:
```
pkg install -y print/freetype2
``` 

Install LCDd:
```
pkg add http://pkg.freebsd.org/freebsd:10:x86:64/release_3/All/lcdproc-0.5.7_2.txz
```
(or if your using a 32-bit system then ```pkg add http://pkg.freebsd.org/freebsd:10:x86:32/release_3/All/lcdproc-0.5.7_2.txz```)
 
Install the LCDproc package for pfsense:
```
pkg add https://github.com/Treer/FreeBSD-ports/releases/download/0.9.15/pfSense-pkg-LCDproc-0.9.15.txz
```

Tell pfsense the package was installed (this would happen automatically if pfsense had installed the package:
```
/usr/local/bin/php -f /etc/rc.packages LCDproc POST-INSTALL
```
