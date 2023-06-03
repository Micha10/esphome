# ESPHome [![Discord Chat](https://img.shields.io/discord/429907082951524364.svg)](https://discord.gg/KhAMKrd) [![GitHub release](https://img.shields.io/github/release/esphome/esphome.svg)](https://GitHub.com/esphome/esphome/releases/)

[![ESPHome Logo](https://esphome.io/_images/logo-text.png)](https://esphome.io/)

**Documentation:** https://esphome.io/

For issues, please go to [the issue tracker](https://github.com/esphome/issues/issues).

For feature requests, please see [feature requests](https://github.com/esphome/feature-requests/issues).


# Changes for Espilight #

## Platformio.ini
```
[common]
lib_deps =
    puuu/ESPiLight@^0.17.0
```

``` pio run build ```

Now the Espilight dependency is downloaded in .pio/libdeps/ in every platform.

But

There is a bug in pilight. If you try to compile, the linker will complain about duplicated symbols. So we have to patch until a fix in pilight will be applied (https://github.com/puuu/ESPiLight/issues/62)

```
In Arduino IDE, with ESP8266 3.0.2 environment, the link errors disappeared after:
	1. Commenting the line #include "protocol_header.h" in the file protocols.c
	2. Commenting the line #include "../../core/dso.h" in all the c files in 433.92 directory (for instance with the command sed -i -e '/dso\.h/s/^/\/\//' *.c
```
We have do this for every platform

