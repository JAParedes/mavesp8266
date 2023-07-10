# MavESP8266

## Current Binary

Download the current version (MAVLink V2) from here: [Firmware version 1.2.2](http://www.grubba.com/mavesp8266/firmware-1.2.2.bin)

Download the legacy version (MAVLink V1) from here: [Firmware version 1.1.1](http://www.grubba.com/mavesp8266/firmware-1.1.1.bin)

## ESP8266 WiFi Access Point and MavLink Bridge

The original code was developed using a [NodeMCU v2 Dev Kit](http://www.seeedstudio.com/depot/NodeMCU-v2-Lua-based-ESP8266-development-kit-p-2415.html). It has been tested with the ESP-01 shipped with the [PixRacer](https://pixhawk.org/modules/pixracer) and it is stable at 921600 baud.

The following build instructions work with Ubuntu 20.04 and the [ESP_01S and Programming Board](https://www.amazon.com/ESP8266-Breakout-Programmer-Downloader-Auto-Download/dp/B08F9X3M5J).

1. Download the [VSCode](https://code.visualstudio.com/) IDE.
1. Open VSCode, and search for the PlatformIO IDE in the VSCode Extension Manager to install it, as shown in [this page](https://platformio.org/install/ide?install=vscode).
1. Install PlatformIO shell commands by following [these instructions](https://docs.platformio.org/en/stable/core/installation/shell-commands.html#piocore-install-shell-commands).
1. Run ```git clone --recursive https://github.com/JAParedes/mavesp8266.git''' and go into the **mavesp8266** directory.
1. Connect the ESP&#95;01S and the Programming Board vis the USB port and run ```pio run -e esp01&#95;1m&#95;MCU -t upload```.

The ESP board will be in access point mode initially. To change its properties, power up the ESP board and connect to the **PixRacer** network with the password **pixracer**. Then, using a web browser, access [http://192.168.4.1](http://192.168.4.1) and click on setup. The description of the available options are shown in [here.](HTTP.md)

### Useful commands:

* ```pio run``` - process/build all targets
* ```pio run -e esp01&#95;1m&#95;MCU``` - process/build just the ESP 01s target (change esp01&#95;1m&#95;MCU for other targets)
* ```pio run -e esp01&#95;1m&#95;MCU -t upload``` - build and upload firmware to embedded board
* ```pio run -t clean``` - clean project (remove compiled files)

The resulting image(s) can be found in the directory ```.pioenvs``` created during the build process.

### Note about Access Point and Station modes:

In Access Point mode, the ESP board is better suited for one-to-one communication between the corresponding PX4 board and a computer, although this may not be suited for applications that require a high rate of data transfer.

In Station Mode, the ESP board can connect to a wireless router, which can increase the range and data throughout, depending on the chosen router. Make sure to choose a suitable IP address and PORT. Note that the ESP board will wait about a minute to connect to the network with the name specified in the **Station SSID** field. After this time, the board will switch to Access Point mode. If you require more time, increase the _120_ number in line 163 in **/src/main.cpp**.

### MavLink Submodule

The ```git clone --recursive``` above not only cloned the MavESP8266 repository but it also installed the dependent [MavLink](https://github.com/mavlink/c_library) sub-module. To upated the module (when needed), use the command ```git submodule update --init'''.

### Wiring it up

User level (as well as wiring) instructions can be found [here for px4](https://docs.px4.io/en/telemetry/esp8266_wifi_module.html) and [here for ArduPilot](http://ardupilot.org/copter/docs/common-esp8266-telemetry.html)

* Resetting to Defaults: In case you change the parameters and get locked out of the module, all the parameters can be reset by bringing the GPIO02 pin low (Connect GPIO02 pin to GND pin). 

### MavLink Protocol

The MavESP8266 handles its own set of parameters and commands. Look at the [PARAMETERS](PARAMETERS.md) page for more information.

### HTTP Protocol

There are some preliminary URLs that can be used for checking the WiFi Bridge status as well as updating firmware and changing parameters. [You can find it here.](HTTP.md)
