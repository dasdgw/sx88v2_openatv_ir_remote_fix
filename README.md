# sx88v2_openatv_ir_remote_fix

## openatv

first create a folder where collect all the repositories.
```
mkdir openatv
cd openatv
```

clone this repo into that folder
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
cd ../openatv7.2/build-enviroment
sed -i "s#git://github.com/openatv/enigma2.git;protocol=https;branch=7.2#git://$(realpath ../../enigma2/);protocol=file;branch=master#g" meta-oe-alliance/meta-oe/conf/distro/openatv.conf
```

to build the image basically follow the instructions under  
https://github.com/openatv/enigma2  
with some minor changes.

- no need to set python3 as the default python interpreter. That's already the case, at least on ubuntu 22.04.
- no need to 'Set your shell to /bin/bash'. (We use a slightly change build cmd)
- no need to add a new user

## build image
you can avoid changing your default sh if you add 'SHELL=bash' to the build cmd.  
So for our stb we get:
```
MACHINE=sx88v2 DISTRO=openatv DISTRO_TYPE=release make enigma2-image SHELL=bash
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
