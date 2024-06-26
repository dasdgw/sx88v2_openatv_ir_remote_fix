# sx88v2_openatv_ir_remote_fix

## openatv

first create a folder where collect all the repositories.
```
mkdir openatv
cd openatv
```

to setup the build environment basically follow the instructions under  
https://github.com/openatv/enigma2  
with some minor changes.

- no need to set python3 as the default python interpreter. That's already the case, at least on ubuntu 22.04.
- no need to 'Set your shell to /bin/bash'. (We use a slightly change build cmd)
- no need to add a new user
- important: skip the last step, for now. This is the actual build command, we will execute it later, after some fourther changes.

Now it's time to clone this repo into our openatv root folder
```
git clone https://github.com/dasdgw/sx88v2_openatv_ir_remote_fix/
```

clone the openatv engima2 repo and apply the patches

```
git clone https://github.com/openatv/enigma2
cd enigma2
git am ../sx88v2_openatv_ir_remote_fix/*.patch
```
now let's configure our local engima2 repo for openatv
```
cd build-enviroment
sed -i "s#git://github.com/openatv/enigma2.git;protocol=https;branch=7.4#git://$(realpath ../../enigma2/);protocol=file;branch=master#g" meta-oe-alliance/meta-oe/conf/distro/openatv.conf
```

## build image
you can avoid changing your default sh if you add 'SHELL=bash' to the build cmd.  
So for our set top box we get:
```
MACHINE=sx88v2 DISTRO=openatv DISTRO_TYPE=release make enigma2-image SHELL=bash
```
If the build succeeded the image can be found under  
builds/openatv/release/sx88v2/tmp/deploy/images/sx88v2/

## install image

```
scp builds/openatv/release/sx88v2/tmp/deploy/images/sx88v2/<image> root@sx88v2:/mnt/hdd/images/
```
receiver gui on tv:

save system settings:  
btn menu -> "Einstellungen" -> "Softwareverwaltung" -> "Systemeinstellungen sichern"

install image:  
btn menu -> "Einstellungen" -> "Softwareverwaltung" -> "Flashen online/lokal" -> "Heruntergeladene Images" ->  

select the new image and follow the on screen instructions

## build package

```
cd builds/openatv/release/sx88v2
source env.source
bitbake -f enigma2
```

## install package

```
scp tmp/deploy/ipk/sx88v2/enigma2_7.2+git32194+9001055-r0_sx88v2.ipk
```

on receiver
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

## update patches
if you have to update the patches:
- commit your changes
- delete the old patches
- recreate the patches
```
cd enigma2
rm ../sx88v2_openatv_ir_remote_fix/000*
git format-patch origin/master..master -o ../sx88v2_openatv_ir_remote_fix/
```
