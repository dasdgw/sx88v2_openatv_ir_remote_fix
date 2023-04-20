# sx88v2_openatv_ir_remote_fix

# openatv

https://github.com/openatv/enigma2

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
