+++
date = 2020-10-15T09:16:58Z
description = ""
draft = false
slug = "remonter-les-dependences-dun-package-debian"
title = "remonter les dépendances d'un package debian"
categories = ["debian"]
+++


En utilisant le paquet `apt-rdepends` (dispo dans les repos par défaut), on est à même de lister les dépendances d'un package et de savoir si les-dites dépendances sont installées. Voyez plutôt :

```bash
$ apt-rdepends -p --state-follow=Installed --state-show=Installed postfix
$ Reading package lists... Done
Building dependency tree
Reading state information... Done
postfix
  Depends: adduser (>= 3.48) [Installed]
  Depends: cpio [Installed]
  Depends: debconf (>= 0.5) [Installed]
  Depends: dpkg (>= 1.8.3) [Installed]
  Depends: init-system-helpers (>= 1.18~) [Installed]
  Depends: libc6 (>= 2.14) [Installed]
  Depends: libdb5.3 [Installed]
  Depends: libicu57 (>= 57.1-1~) [Installed]
  Depends: libsasl2-2 [Installed]
  Depends: libssl1.1 (>= 1.1.0) [Installed]
  Depends: lsb-base (>= 3.0-6) [Installed]
  Depends: netbase [Installed]
  Depends: ssl-cert [Installed]
adduser
  Depends: debconf (>= 0.5) [Installed]
  Depends: passwd [Installed]
debconf
  PreDepends: perl-base (>= 5.20.1-3~) [Installed]
perl-base
  PreDepends: dpkg (>= 1.17.17) [Installed]
  PreDepends: libc6 (>= 2.23) [Installed]
dpkg
  Depends: tar (>= 1.28-1) [Installed]
  PreDepends: libbz2-1.0 [Installed]
  PreDepends: libc6 (>= 2.14) [Installed]
  PreDepends: liblzma5 (>= 5.2.2) [Installed]
  PreDepends: libselinux1 (>= 2.3) [Installed]
  PreDepends: zlib1g (>= 1:1.1.4) [Installed]
tar
  PreDepends: libacl1 (>= 2.2.51-8) [Installed]
  PreDepends: libc6 (>= 2.17) [Installed]
  PreDepends: libselinux1 (>= 1.32) [Installed]
libacl1
  Depends: libattr1 (>= 1:2.4.46-8) [Installed]
  Depends: libc6 (>= 2.14) [Installed]
libattr1
  Depends: libc6 (>= 2.4) [Installed]
libc6
  Depends: libgcc1 [Installed]
libgcc1
  Depends: gcc-6-base (= 6.3.0-18+deb9u1) [Installed]
  Depends: libc6 (>= 2.14) [Installed]
gcc-6-base
libselinux1
  Depends: libc6 (>= 2.14) [Installed]
  Depends: libpcre3 [Installed]
libpcre3
  Depends: libc6 (>= 2.14) [Installed]
  PreDepends: multiarch-support [Installed]
multiarch-support
  Depends: libc6 (>= 2.3.6-2) [Installed]
libbz2-1.0
  Depends: libc6 (>= 2.4) [Installed]
liblzma5
  Depends: libc6 (>= 2.17) [Installed]
zlib1g
  Depends: libc6 (>= 2.14) [Installed]
passwd
  Depends: libaudit1 (>= 1:2.2.1) [Installed]
  Depends: libc6 (>= 2.14) [Installed]
  Depends: libpam-modules [Installed]
  Depends: libpam0g (>= 0.99.7.1) [Installed]
  Depends: libselinux1 (>= 1.32) [Installed]
  Depends: libsemanage1 (>= 2.0.3) [Installed]
libaudit1
  Depends: libaudit-common (>= 1:2.6.7-2) [Installed]
  Depends: libc6 (>= 2.14) [Installed]
  Depends: libcap-ng0 [Installed]
libaudit-common
libcap-ng0
  Depends: libc6 (>= 2.8) [Installed]
libpam-modules
  PreDepends: debconf (>= 0.5) [Installed]
  PreDepends: libaudit1 (>= 1:2.2.1) [Installed]
  PreDepends: libc6 (>= 2.15) [Installed]
  PreDepends: libdb5.3 [Installed]
  PreDepends: libpam-modules-bin (= 1.1.8-3.6) [Installed]
  PreDepends: libpam0g (>= 1.1.3-2) [Installed]
  PreDepends: libselinux1 (>= 2.1.9) [Installed]
libdb5.3
  Depends: libc6 (>= 2.17) [Installed]
libpam-modules-bin
  Depends: libaudit1 (>= 1:2.2.1) [Installed]
  Depends: libc6 (>= 2.14) [Installed]
  Depends: libpam0g (>= 0.99.7.1) [Installed]
  Depends: libselinux1 (>= 1.32) [Installed]
libpam0g
  Depends: debconf (>= 0.5) [Installed]
  Depends: libaudit1 (>= 1:2.2.1) [Installed]
  Depends: libc6 (>= 2.14) [Installed]
libsemanage1
  Depends: libaudit1 (>= 1:2.2.1) [Installed]
  Depends: libbz2-1.0 [Installed]
  Depends: libc6 (>= 2.14) [Installed]
  Depends: libselinux1 (>= 2.6) [Installed]
  Depends: libsemanage-common (= 2.6-2) [Installed]
  Depends: libsepol1 (>= 2.6) [Installed]
  Depends: libustr-1.0-1 (>= 1.0.4) [Installed]
libsemanage-common
libsepol1
  Depends: libc6 (>= 2.14) [Installed]
libustr-1.0-1
  Depends: libc6 (>= 2.14) [Installed]
cpio
  Depends: libc6 (>= 2.17) [Installed]
init-system-helpers
  Depends: perl-base (>= 5.20.1-3) [Installed]
libicu57
  Depends: libc6 (>= 2.14) [Installed]
  Depends: libgcc1 (>= 1:3.0) [Installed]
  Depends: libstdc++6 (>= 5.2) [Installed]
libstdc++6
  Depends: gcc-6-base (= 6.3.0-18+deb9u1) [Installed]
  Depends: libc6 (>= 2.18) [Installed]
  Depends: libgcc1 (>= 1:4.2) [Installed]
libsasl2-2
  Depends: libc6 (>= 2.15) [Installed]
  Depends: libsasl2-modules-db (>= 2.1.27~101-g0780600+dfsg-3+deb9u1) [Installed]
libsasl2-modules-db
  Depends: libc6 (>= 2.14) [Installed]
  Depends: libdb5.3 [Installed]
libssl1.1
  Depends: debconf (>= 0.5) [Installed]
  Depends: libc6 (>= 2.17) [Installed]
lsb-base
netbase
ssl-cert
  Depends: adduser [Installed]
  Depends: debconf (>= 0.5) [Installed]
  Depends: openssl (>= 0.9.8g-9) [Installed]
openssl
  Depends: libc6 (>= 2.15) [Installed]
  Depends: libssl1.1 (>= 1.1.0) [Installed]
```

Il n'y a plus qu'à faire la liste !



