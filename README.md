# LCDproc for pfSense #

This fork of [pfsense/FreeBSD-ports](https://github.com/pfsense/FreeBSD-ports) is where I'm updating my LCDproc package to be compatible with pfSense 2.3 onward.

The ports package [has been created](https://github.com/Treer/FreeBSD-ports/tree/devel/sysutils/pfSense-pkg-LCDproc) and [compiled](https://github.com/Treer/FreeBSD-ports/releases) from the LCDproc**-dev** package of pfSense 2.2. I chose LCDproc-dev because it's had more work and functionality added than LCDproc, and was first created to add early support for LCDd 0.5.4 without affecting pfSense's LCDproc package. So since a pfSense 2.3+ port will be using LCDd 0.5.7+ anyway, there's no need to keep maintaining two packages.


* Remaining tasks:

    ☑ Put the existing LCDproc files into a pfSense 2.3 compatible ports package *([Now implemented](https://github.com/Treer/FreeBSD-ports/releases) in 0.9.5)*

    ☑ Update UI to use Bootstrap *([Now implemented](https://github.com/Treer/FreeBSD-ports/releases) in 0.10.0)*

    ☑ Add realtime traffic features to LCDproc, so I can know who's using the damn bandwidth ;) *([Now implemented](https://github.com/Treer/FreeBSD-ports/releases) in 0.10.1)*
    
    ☑ Change the interface totals screen to be daily totals instead *([Now implemented](https://github.com/Treer/FreeBSD-ports/releases) in 0.10.2)*

    ☑ Submit it as [a pull request to pfSense](https://github.com/pfsense/FreeBSD-ports/pull/178)
    

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
    pkg add https://github.com/Treer/FreeBSD-ports/releases/download/0.10.4-amd64/pfSense-pkg-LCDproc-0.10.4.txz
    ```
    or, if you use a 32-bit system:
    ```
    pkg add https://github.com/Treer/FreeBSD-ports/releases/download/0.10.4-i386/pfSense-pkg-LCDproc-0.10.4.txz
    ```

1.  Tell pfsense the package was installed (this would happen automatically if pfsense had installed the package):
    ```
    /usr/local/bin/php -f /etc/rc.packages LCDproc POST-INSTALL
    ```
    You'll also have to insert the text below into the `<installedpackages>` tag inside `/conf/config.xml`, and reboot:
    ```
    <menu>
        <name>LCDproc</name>
        <tooltiptext>Set LCDproc settings such as display driver and COM port.</tooltiptext>
        <section>Services</section>
        <url>/packages/lcdproc/lcdproc.php</url>
    </menu>
    <service>
        <name>lcdproc</name>
        <rcfile>lcdproc.sh</rcfile>
        <executable>LCDd</executable>
        <description><![CDATA[LCD Driver]]></description>
    </service>
    ```
    
    **Note** the `<url>/packages/lcdproc/lcdproc.php</url>` in the `<menu>` tag is different to that of the old LCDproc package.
    
    An alternate way to do this is *Diagnostics* **→** *Backup & Restore*, select "Package Manager" in the dropdown, then click *Download configuration as XML*. Edit the configuration and upload it. This way saves needing to reboot.

**Troubleshooting** - If you've done all that **and rebooted** and LCDproc still isn't available from the Services menu...
* You should be able to reach the LCDproc config screens by navigating your browser to `/packages/lcdproc/lcdproc.php` - this is where the menu would link to.
* I've found that using the package manager to install a small package like Cron can sort out the LCDproc menu, and you can always remove Cron again afterwards. The *Diagnostics* **→** *Backup & Restore* method for editing the config (detailed above) might also fix this.

Installing another package to trigger the package manager suggests to me that step 4 might not be working, or I've overlooked something the package manager does for you. If anybody knows, let me know.

## Update ##

Still working this out. Uninstall:

1. ```/usr/local/bin/php -f /etc/rc.packages LCDproc DEINSTALL```
2. ```pkg remove pfSense-pkg-LCDproc-0.9.15```
3. ```/usr/local/bin/php -f /etc/rc.packages LCDproc POST-DEINSTALL```
4. [This step won't be required if you were already on LCDproc version 0.10.x] Remove lcdproc sections from the `<installedpackages>` tag inside `/conf/config.xml`, and reboot.

Then jump to step 3 of the installation instructons using the version you want to update to.

## Licence ##

All my contributions can be considered as available under the ESF license.
