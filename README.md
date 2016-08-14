# LCDproc for pfSense #

This fork of [pfsense/FreeBSD-ports](https://github.com/pfsense/FreeBSD-ports) is where I'm updating my LCDproc package to be compatible with pfSense 2.3 onward.

The ports package [has been created](https://github.com/Treer/FreeBSD-ports/tree/devel/sysutils/pfSense-pkg-LCDproc) and [compiled](https://github.com/Treer/FreeBSD-ports/releases) from the LCDproc**-dev** package of pfSense 2.2. I chose LCDproc-dev because it's had more work and functionality added than LCDproc, and was first created to add early support for LCDd 0.5.4 without affecting pfSense's LCDproc package. So since a pfSense 2.3+ port will be using LCDd 0.5.7+ anyway, there's no need to keep maintaining two packages.


Remaining tasks:
* Add realtime traffic features to LCDproc
* Update UI to use Bootstrap

## Installation ##

You can type these commands into the web GUI, via *Diagnostics* **→** *Command Prompt* **→** *Execute Shell Command*

(But don't be doing it on a production system)

1.  Install the freetype2 library:

    ```
    pkg install -y print/freetype2
    ``` 

1.  Install LCDd (*"LCDd"* is what I'm calling BSD's LCDproc to reduce ambiguity):
    ```
    pkg add http://pkg.freebsd.org/freebsd:10:x86:64/release_3/All/lcdproc-0.5.7_2.txz
    ```
    or, if you use a 32-bit system:
    ```
    pkg add http://pkg.freebsd.org/freebsd:10:x86:32/release_3/All/lcdproc-0.5.7_2.txz
    ```
 
1.  Install the LCDproc package for pfsense:
    ```
    pkg add https://github.com/Treer/FreeBSD-ports/releases/download/0.9.15/pfSense-pkg-LCDproc-0.9.15.txz
    ```

1.  Tell pfsense the package was installed (this would happen automatically if pfsense had installed the package):
    ```
    /usr/local/bin/php -f /etc/rc.packages LCDproc POST-INSTALL
    ```
    You'll also have to insert the text below into the `<installedpackages>` tag inside `/conf/config.xml', and reboot:
    ```
    <menu>
        <name>LCDproc</name>
        <tooltiptext>Set LCDproc settings such as display driver and COM port.</tooltiptext>
        <section>Services</section>
        <url>/packages/lcdproc/lcdproc.php</url>
    </menu>
    ```
    An alternate way to do this is *Diagnostics* **→** *Backup & Restore*, select "Package Manager" in the dropdown, then click *Download configuration as XML*. Edit the configuration and upload it. This way saves needing to reboot.

## Update ##

Still working this out. Uninstall:

1. ```/usr/local/bin/php -f /etc/rc.packages LCDproc DEINSTALL```
2. ```pkg remove pfSense-pkg-LCDproc-0.9.15```
3. ```/usr/local/bin/php -f /etc/rc.packages LCDproc POST-DEINSTALL```

Then jump to step 3 of the installation instructons using the version you want to update to.

## Licence ##

All my contributions can be considered as available under the ESF license. 

(I assume pfSense will have no interest in any of this until/unless there's a full Bootstrap coversion and signed CLAs from every contributor)
