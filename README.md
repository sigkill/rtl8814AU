# rtl8814AU
Realtek 8814AU USB WiFi driver.

Forked from [Diederik de Haas](https://github.com/diederikdehaas/rtl8814AU)'
repository which is based on version 4.3.21 of an Edimax driver for the
EW-7833UAC device.

Updated with support for kernels >= 4.14.

# Caveat Emptor

This code is in maintenance mode; it is kept up to date with the Linux kernel
as far as possible, but no changes are made to the code other than that.

If you are thinking of buying a product with this chip in: don't, unless you
are prepared to put a considerable amount of work in yourself. More generally,
don't buy any hardware that is not supported by the mainline kernel. Some
companies claim "Linux support" on the box, on the basis that some third party
hosts some (likely unsupported and unmaintained) driver source code for an
antiquated kernel version. If you have the choice, look for
[better alternatives](https://wireless.wiki.kernel.org/en/users/drivers).

# Manual build

When building the code, you need to point `make` to the kernel source and
specify the kernel version.
```shell
make install KSRC=/path/to/linux-source KVER=x.y.x
```

This script automates this:
```shell
#!/bin/sh
set -e

src=/usr/src/linux
suffix=$(sed -ne 's/^CONFIG_LOCALVERSION="\(.*\)"/\1/p' $src/.config)

ver=$(cd $src; make kernelversion)$suffix
make -j clean all KSRC=$src KVER=$ver
sudo make install KSRC=$src KVER=$ver
```

# DKMS support

From your src dir

````shell
sudo cp -R . /usr/src/rtl8814au-4.3.21
sudo dkms build -m rtl8814au -v 4.3.21
sudo dkms install -m rtl8814au -v 4.3.21
````

This should keep your 8814AU adapter working post kernel updates.
