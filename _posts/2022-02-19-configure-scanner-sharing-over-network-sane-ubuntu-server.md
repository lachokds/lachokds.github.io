---
layout: post
title: "Configure Scanner Sharing Over the Network Using SANE on Ubuntu Server"
date: 2022-02-19 23:58:00
categories: printer printing cups ubuntu-server cups-ubuntu pixma canon
---

## Installation

Install `ipp-usb` snap with

```
sudo snap install ipp-usb --edge
```

If you don't use the `--edge` flag, you will get a message like this:

```
$ sudo snap install ipp-usb
error: snap "ipp-usb" is not available on stable but is available to install on the following
       channels:

       edge       snap install --edge ipp-usb

       Please be mindful pre-release channels may include features not completely tested or
       implemented. Get more information with 'snap info ipp-usb'.
```

Install `sane` and `sane-utils` packages with

```
$ sudo apt install sane sane-utils
```

`saned` provides the SANE daemon, which is the system service
`sane-utils`  includes the command line frontend `scanimage`, the saned server and the `sane-find-scanner` utility, along with their documentation

Once installed, we can check if the scanner is detected  by turning it on and running the `scanimage -L` command **with `sudo`**:

```
$ sudo scanimage -L
...
device `pixma:04A91728_14FE41' is a CANON Canon PIXMA MX310 multi-function peripheral
```

As you can see, my Canon PIXMA MX310 was successfully detected.

If you take a look at the `/etc/passwd` and `/etc/group` files, you'll notice that we now have a `saned` user and a couple of groups, `scanner` and `saned`:

```
$ cat /etc/passwd
...
saned:x:113:123::/var/lib/saned:/usr/sbin/nologin
```

```
$ cat /etc/group
...
scanner:x:122:saned
...
saned:x:123:
```

This is because, by default, the `saned` daemon is configured to run as the `saned` user, which we can check by looking at the contents of the `/etc/default/saned` file:

```
$ cat /etc/default/saned
# Defaults for the saned initscript, from sane-utils

# Set to the user saned should run as
RUN_AS_USER=saned
```

Now, we need to configure `sane` to run as a server. To do so, add the following lines to `/etc/default/sane`, preferrably after the `Defaults for the saned initscript` comment:

```
# Set to yes to start saned
RUN=yes
```

In fact, you only need the `RUN=yes` part, but it's always a good idea to have a comment that tells you what you're configuring

Once you've saved the file, restart saned with the following command:

```
$ sudo systemctl restart saned.socket
```

## Set user permissions

As we saw earlier, the `saned` daemon runs as its own user. However, to share the scanner over the network, we need to give that user the appropriate permissions to access the scanner.

We will do this by creating a `udev` rule that gives us that permisssion. But first, we need to know the `vendorID` and `productID` of our scanner.

To find those, run `sane-find-scanner` with root permissions:

>$ sudo sane-find-scanner
>
>...
>
>found USB scanner (vendor=**0x04a9** \[Canon\], product=**0x1728** \[MX310 series\]) at libusb:001:007
>found USB scanner (vendor=**0x0424**, product=**0xec00**) at libusb:001:003

If you get more that one scanner, as in this example, you can compare that to the output of `lsusb`, to find which IDs match your scanner

```
$ lsusb
Bus 001 Device 007: ID 04a9:1728 Canon, Inc. PIXMA MX310 series
...
```

As you can see, the very first one is our match. Now we can create our `udev` rule which should look something like:

> ATTRS{idVendor}\=\="**vendorID**", ATTRS{idProduct}\=\="**productID**", MODE="0664", GROUP="lp", ENV{libsane_matched}="yes"

To simplify things for yourself, you could also use the `echo` command as *root*, remembering to substitute with *your own* **vendorID** and **productID**:

```
sudo su

echo 'ATTRS{idVendor}=="0x04a9", ATTRS{idProduct}=="0x1728", MODE="0664", GROUP="lp", ENV{libsane_matched}="yes"' > /etc/udev/rules.d/49-sane-missing-scanner.rules
```

Now, still as *root*, add the `saned` user tot he `lp` group using `gpasswd`:

```
gpasswd -a saned lp
```

Then, plug the scanner out and plug it back in, and the file permissions should now be correect.

To verify, use the following command to start a shell as the `saned` user:

```
# su -s /bin/bash saned
```

And then, test with `scanimage -L`

```
saned@server$ scanimage -L
...
device `pixma:04A91728_14FE41' is a CANON Canon PIXMA MX310 multi-function peripheral
```

## Share your scanner over the network

To share your scanner, we begin by configuring the machine that will act as the "print server", to allow hosts on my network to access the printer.

Change the `/etc/sane.d/saned.conf` file with your preferences. For example, my network uses the `192.168.10.0/24` address range with a `/24` subnet, which means that, under the `## Access list` section, my configuration looks like this:

```
## Access list
...
localhost
192.168.10.0/24
```

Now, **enable** and **restart** `saned.socket` to restart the daemon.

```
$ sudo systemctl enable saned.socket
$ sudo systemctl restart saned.socket
```

Your scanner is now available over the network. If you use a desktop environment, you should be able to find it using the `Add/Remove` printers section of your distribution, as long as it has a graphical frontend like `simple-scan`, `gscan2pdf` or `xsane`.

## Client Host configuration

If, like me, you don't use a graphical Desktop Environment, but instead a window manager like `dwm` (shameless shilling, I know :-P), you'll need to install `sane` and any specific utility similar to `sane-airscan` that allows your computer to connect to the external scanner that is available over the network.

Next, you'll need to tell `sane` the location of your print server, by editing the `/etc/sane.d/net.conf` file with root privileges.

You'll need to include the IP address of your print server. If you also have mDNS hostname resolution set up through `avahi`, it's also a great idea to include that as well, like so:

```
$ sudo vim /etc/sane.d/net.conf
```

```
...
HOSTNAME.local
192.168.10.X
```

Now, on the Client host, run `scanimage -L`

```
$ scanimage -L
...
device `net:192.168.10.X:pixma:04A91728_14FE41' is a CANON Canon PIXMA MX310 multi-function peripheral
```

From here, you can fire up your scanning graphical frontend, and you _should_ be able to scan any document remotely.
