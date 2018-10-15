# RedisDesktopManager-Linux-WSL
Base on WSL(Windows Subsystem for Liunx)

# Before
- WSL = Windows Subsystem for Liunx
- WSL not support `snap`, see [link](https://forum.snapcraft.io/t/windows-subsystem-for-linux/216/10#post_10)

# Installation

```bash
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

---

# Some issues
## c++11
```bash
apt install g++
```

## Can't clone lss
Solution: IGNORE, just `qmake`


## Missing `libbreakpad_client.a`
Error
```bash
g++: error: /data/RedisDesktopManager/3rdparty/gbreakpad/src/client/linux/libbreakpad_client.a: No such file or directory
Makefile:564: recipe for target '../bin/linux/release/rdm' failed
```
Solution
```bash
cd RedisDesktopManager/3rdparty/gbreakpad/
./configure
make
```

## Maybe can fix some missing methods
```bash
apt install zlib1g-dev
```


---

# Optional, Add to PATH
Easy use
```bash
# RDM
export RDM_HOME=/opt/redis-desktop-manager
export PATH=$RDM_HOME:$PATH
```



