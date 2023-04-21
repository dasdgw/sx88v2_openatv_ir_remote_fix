# sx88v2_openatv_ir_remote_fix

## openatv

basically follow the instructions under  
https://github.com/openatv/enigma2  
with some minor changes.

- no need to set python3 as the default python interpreter. That's already the case, at least on ubuntu 22.04.
- TODO check if 'Set your shell to /bin/bash' is necessary (feedback welcome)
- no need to add a new user

## local repo

https://forums.openpli.org/topic/41127-how-to-build-openpli-with-custom-patches/?view=findpost&p=540969

## apply patches to local repo of enigma2
```
git-am *.patch
```
## build image
```
MACHINE=sx88v2 DISTRO=openatv DISTRO_TYPE=release make enigma2-image
```
If the build succeeded the image can be found under  
builds/openatv/release/sx88v2/tmp/deploy/images/sx88v2/

## build package

```
cd builds/openatv/release/sx88v2
source env.source
bitbake -f enigma2
```

```
scp tmp/deploy/ipk/sx88v2/enigma2_7.2+git32194+9001055-r0_sx88v2.ipk
```

auf receiver
```
opkg install /tmp/enigma2_7.2+git32194+ab2f051-r0_sx88v2.ipk --force-overwrite --force-downgrade
```
## debug log
```
init 5 && sleep 10 && ENIGMA_DEBUG_LVL=6 enigma2 2>&1 | tee debug.log
```

when done
```
init 3
```

## ir remote control

SX RCU 03  
handled by hisi-ir.ko?  
closed source module found in openatv source folder
