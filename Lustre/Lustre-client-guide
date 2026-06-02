# Installing Lustre while respecting the current kernel

This is a quick installation guide for Lustre on a Red Hat-based system, specifically Rocky Linux 10.

## Check/update the current kernel

```bash
cat /etc/rocky-release
uname -r
dnf list installed 'kernel*' | cat
dnf repolist
```

```text
Rocky Linux release 10.1 (Red Quartz)
Initial kernel: 6.12.0-124.56.1.el10_1.x86_64
```

```bash
dnf update -y kernel kernel-core kernel-modules kernel-modules-core kernel-modules-extra
dnf install -y kernel-devel kernel-headers kernel-devel-matched
reboot
```

## Check that the kernel has been updated

```bash
uname -r
rpm -q kernel-devel-$(uname -r)
```

## Install dependencies

```bash
dnf install -y \
  gcc make perl patch git wget \
  rpm-build redhat-rpm-config \
  keyutils keyutils-libs-devel \
  libmount-devel libnl3-devel libnl3-cli \
  elfutils-libelf-devel openssl-devel \
  zlib-devel \
  autoconf automake libtool \
  kernel-rpm-macros \
  kernel-devel-$(uname -r) kernel-headers libyaml-devel
```

## YAML usually causes an error. If it does, use these commands

```bash
dnf install -y dnf-plugins-core
dnf config-manager --set-enabled crb
dnf install -y libyaml-devel
```

## Git

```bash
cd /usr/local/src
git clone https://github.com/lustre/lustre-release.git
cd lustre-release
git checkout master
git pull
```

## Compile

```bash
sh autogen.sh
./configure --disable-server --with-linux=/usr/src/kernels/$(uname -r)
make rpms -j$(nproc)
```

## If it gives an error, install the missing dependencies. In my case

```bash
dnf install -y kernel-rpm-macros
make rpms -j$(nproc)
```

## Install the Lustre RPMs

```bash
dnf install -y \
/usr/local/src/lustre-release/kmod-lustre-client-2.17.53_56_g69a0a33-1.el10.x86_64.rpm \
/usr/local/src/lustre-release/lustre-client-2.17.53_56_g69a0a33-1.el10.x86_64.rpm \
/usr/local/src/lustre-release/lustre-client-iokit-2.17.53_56_g69a0a33-1.el10.x86_64.rpm
```

## LNet

This makes Lustre work through the specified network port.

```bash
modprobe lnet
lctl network up
sudo lnetctl net add --net tcp --if eno4np1
```

## Create the mount folder and mount Lustre

```bash
mkdir -p /mnt/smart
mount -t lustre -o flock 192.168.200.70@tcp:/smart /mnt/smart
```

---

Copyright © 2026 Pau Ubach

This document is licensed under the GNU Free Documentation License v1.3 or later.

SPDX-License-Identifier: GFDL-1.3-or-later
