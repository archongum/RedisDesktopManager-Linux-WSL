
# RedisDesktopManager-Linux-WSL
Base on WSL(Windows Subsystem for Linux)

**Tested on Ubuntu 18.04 LTS**

# Before
- WSL = Windows Subsystem for Liunx
- WSL not support `snap`, see [link](https://forum.snapcraft.io/t/windows-subsystem-for-linux/216/10#post_10)

# Installation

```bash
# need to upgrade
apt update && apt upgrade

# WSL has not g++
apt install g++

# change to latest tag
git clone --recursive https://github.com/uglide/RedisDesktopManager.git -b 0.9.8

cd RedisDesktopManager/
cd src/
./configure
qmake
make
make install

cd /opt/redis-desktop-manager/
mv qt.conf qt.backup

# optional, reduces rdm size 30MB+ to 2MB+
strip rdm
```

# Install GUI package
```bash
apt install xfce4
```

# Some issues
## Set default user to `root`
Open `PowerShell`: <kbd>win</kbd>+<kbd>x</kbd>, <kbd>a</kbd>
```bash
ubuntu1804.exe config --default-user root
```

## g++(c++11)
```bash
apt install g++
```

## Unable to localte package qt5
Error
```bash
E: Unable to locate package qt5-default
E: Unable to locate package qtdeclarative5-dev
E: Unable to locate package libqt5charts5-dev
E: Unable to locate package qml-module-qtquick-controls
E: Unable to locate package qml-module-qtquick-controls2
E: Unable to locate package qml-module-qt-labs-settings
E: Unable to locate package qml-module-qtquick-dialogs
E: Unable to locate package qml-module-qtquick-layouts
E: Unable to locate package qml-module-qt-labs-folderlistmodel
E: Unable to locate package qml-module-qtqml-models2
E: Unable to locate package qml-module-qtcharts
```
Solution
```bash
apt update && apt upgrade
```

## Cannot clone linux-syscall-support repo(because of Great Firewall)
Appear in `./configure`

Error
```bash
Cloning into 'src/third_party/lss'...
fatal: unable to access 'https://chromium.googlesource.com/linux-syscall-support/': Failed to connect to chromium.google
source.com port 443: Connection refused
Cannot clone linux-syscall-support repo
```
Solution: **IGNORE IT**

## Failure to find: app/logger.h
Appear in `qmake`

Error
```bash
WARNING: Failure to find: app/logger.h
```

Solution: **IGNORE IT**

## third_party/lss/linux_syscall_support.h: No such file
Appear in `make`, because of Great Firewall, can't clone `https://chromium.googlesource.com/linux-syscall-support/` repo

Error
```bash
In file included from ../3rdparty/gbreakpad/src/client/linux/dump_writer_common/thread_info.h:37:0,                 
from ../3rdparty/gbreakpad/src/client/linux/minidump_writer/linux_dumper.h:51,                 
from ../3rdparty/gbreakpad/src/client/linux/minidump_writer/minidump_writer.h:41,                 
from ../3rdparty/gbreakpad/src/client/linux/handler/exception_handler.h:42,                 
from modules/crashhandler/crashhandler.cpp:10:../3rdparty/gbreakpad/src/common/memory_allocator.h:50:10: 
fatal error: third_party/lss/linux_syscall_support.h: No such file or directory 
#include "third_party/lss/linux_syscall_support.h"          
compilation terminated.Makefile:1662: recipe for target '../bin/linux/release/obj/crashhandler.o' failedmake: *** [../bin/linux/release/obj/crashhandler.o] Error 1
```

Solution
> Download `linux_syscall_support.h` file 
> 链接：https://pan.baidu.com/s/19jQK3S16ZvUOUzPAtRZujg 
> 提取码：odns
```bash

mkdir /opt/RedisDesktopManager/3rdparty/gbreakpad/src/
third_party/lss

cp linux_syscall_support.h /opt/RedisDesktopManager/3rdparty/gbreakpad/src/
third_party/lss
```



## Missing `libbreakpad_client.a`
Appear in `make`

Error
```bash
g++: error: /data/RedisDesktopManager/3rdparty/gbreakpad/src/client/linux/libbreakpad_client.a: No such file or directory
Makefile:564: recipe for target '../bin/linux/release/rdm' failed
```
Solution
```bash
cd /opt/RedisDesktopManager/3rdparty/gbreakpad/
./configure
make
```

## /usr/bin/ld: cannot find -l*
Appear in `make`

Error
```bash
/usr/bin/ld: cannot find -lssh2
/usr/bin/ld: cannot find -lz
collect2: error: ld returned 1 exit status
Makefile:465: recipe for target '../bin/linux/release/rdm' failed
make: *** [../bin/linux/release/rdm] Error 1
```
Solution
```bash
# for -lssh2
apt install libssh2-1-dev
# for -lz
apt install zlib1g-dev
# for other, Google: /usr/bin/ld: cannot find -l<other package>
```

# Optional, Add to PATH
Easy use
```bash
# RDM
export RDM_HOME=/opt/redis-desktop-manager
export PATH=$RDM_HOME:$PATH
```



