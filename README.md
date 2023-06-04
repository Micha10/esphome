# ESPHome [![Discord Chat](https://img.shields.io/discord/429907082951524364.svg)](https://discord.gg/KhAMKrd) [![GitHub release](https://img.shields.io/github/release/esphome/esphome.svg)](https://GitHub.com/esphome/esphome/releases/)

[![ESPHome Logo](https://esphome.io/_images/logo-text.png)](https://esphome.io/)

**Documentation:** https://esphome.io/

For issues, please go to [the issue tracker](https://github.com/esphome/issues/issues).

For feature requests, please see [feature requests](https://github.com/esphome/feature-requests/issues).


# Changes in this repo for Espilight #

Espilight is a wrapper to be able to compile the pilight protocols with an ESP.

But there is a bug in pilight. If you try to compile it, the linker will complain about duplicated symbols. So we have to patch the code until a fix in pilight will be applied (https://github.com/puuu/ESPiLight/issues/62)

```
In Arduino IDE, with ESP8266 3.0.2 environment, the link errors disappeared after:
	1. Commenting the line #include "protocol_header.h" in the file protocols.c
	2. Commenting the line #include "../../core/dso.h" in all the c files in 433.92 directory (for instance with the command sed -i -e '/dso\.h/s/^/\/\//' *.c)
```

In platformio.ini you *would* add a dependency for espilight
```
[common]
lib_deps =
    puuu/ESPiLight@^0.17.0
```
Then you have to build once to download the dependency. Then you have to patch the code and do another build. Since a build lasts over an hour on my notebook, I've decided to upload a patched version of espilight. 

In the original code pilight is added as a git submodule. I've removed the submodule and added the patched code.

## How to compile
```
git clone https://github.com/esphome/esphome
script/setup
chmod 777 venv/bin/activate
source venv/bin/activate
```
In Platformio.ini add a dependency with the patched pilight
```
[common]
lib_deps =
    https://github.com/Micha10/ESPiLight.git
```

```pio run```

(Compiling in a vm didn't work often. As described in https://github.com/esphome/issues/issues/2206 I had to give more and more memory to the vm and the build lasts long, so I've installed linux on a notebook and never had these compiling issues again)

