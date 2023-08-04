# DRONE-BRIDGE-TELEMTRY-ESP32

![DroneBridgeLogo_text](https://github.com/VISHNUPRIYANR1812/DRONE-BRIDGE-TELEMTRY-ESP32/assets/134359531/04660b4f-919c-4a80-8d0b-7da2e5a081b0)


# DroneBridge for ESP32
A DroneBridge enabled firmware for the popular ESP32 modules from Espressif Systems. Probably the cheapest way to
communicate with your drone, UAV, UAS, ground based vehicle or whatever you may call them.

It also allows for a fully transparent serial to wifi pass through link with variable packet size
(Continuous stream of data required).

DroneBridge for ESP32 is a telemetry/low data rate only solution. There is no support for cameras connected to the ESP32 since it does not support video encoding.

![db_ESP32_setup](https://github.com/VISHNUPRIYANR1812/DRONE-BRIDGE-TELEMTRY-ESP32/assets/134359531/8526d4c7-692c-4fee-a5bf-8661bbbeb1ee)


## Features
-   Bi-directional link: MAVLink, MSP & LTM
-   Affordable: ~7€
-   Up to 150m range
-   Weight: <10 g
-   Supported by: DroneBridge for Android (app), mwptools, QGroundControl, impload etc.
-   Easy to set up: Power connection + UART connection to flight controller
-   Fully configurable through easy to use web interface
-   Parsing of LTM & MSPv2 for more reliable connection and less packet loss
-   Fully transparent telemetry downlink option for continuous streams like MAVLink or and other protocol
-   Reliable, low latency, light weight
-   Upload mission etc.

![ESP32 module with VCP](https://upload.wikimedia.org/wikipedia/commons/thumb/2/20/ESP32_Espressif_ESP-WROOM-32_Dev_Board.jpg/313px-ESP32_Espressif_ESP-WROOM-32_Dev_Board.jpg)

![DroneBridgeForESP32Blackbox](https://github.com/VISHNUPRIYANR1812/DRONE-BRIDGE-TELEMTRY-ESP32/assets/134359531/7daed05e-b766-48d6-853b-a415cb7ff7db)


Blackbox concept. UDP & TCP connections possible. Automatic UDP uni-cast of messages to port 14550 to all 
connected devices/stations. Allows additional clients to register for UDP. Client must send a packet with length > 0 to UDP port of ESP32.

## Hardware

All ESP32 development boards will work. No additional PSRAM required. You will need a USB to serial adapter if for flashing the firmware, if your ESP32 board does not come with one. Follow the instructions of the board manufacturer when it comes to wiring the power supply lines. Some modules do not like an external 5V power input connected in addition to an USB at the same time.

Examples for boards that will work:
* AZDelivery DevKit C
* [TinyPICO - ESP32 Development Board - V2](https://www.adafruit.com/product/4335)
* [Adafruit HUZZAH32 – ESP32 Feather Board](https://www.adafruit.com/product/3405)
* [Adafruit AirLift – ESP32 WiFi Co-Processor Breakout Board](https://www.adafruit.com/product/4201) (requires FTDI adapter for flashing firmware)
* [Adafruit HUZZAH32](https://www.adafruit.com/product/4172) (requires FTDI adapter for flashing firmware)

DroneBridge for ESP32 is tested with an DOIT ESP32 development board.


## Installation/Flashing using precompiled binaries

First download the latest release from this repository.
[You can find them here](https://github.com/DroneBridge/ESP32/releases).

There are many multiple ways on how to flash the firmware. The easy ones are explained below.

**Erase the flash before flashing a new release!**

#### All platforms: Use Espressif firmware flashing tool

#### Recommended

1.  [Download the esp-idf for windows](https://docs.espressif.com/projects/esp-idf/en/release-v4.3/esp32/get-started/windows-setup.html#get-started-windows-tools-installer) or [linux](https://docs.espressif.com/projects/esp-idf/en/release-v4.3/esp32/get-started/linux-setup.html) or install via `pip install esptool`
2.  Connect via USB/Serial. Find out the serial port via `dmesg` on linux or using the device manager on windows.
  In this example the serial connection to the ESP32 is on `COM4` (in Linux e.g. `/dev/ttyUSB0`).
3. `esptool.py -p COM4 erase_flash`
4. ```shell
   esptool.py -p COM4 -b 460800 --before default_reset --after hard_reset --chip esp32  write_flash --flash_mode dio --flash_size detect --flash_freq 40m 0x1000 bootloader.bin 0x8000 partition-table.bin 0x10000 db_esp32.bin 0x110000 www.bin
   ```
   You might need to press the boot button on your ESP to start the upload/flash process.

[Look here for more detailed information](https://github.com/espressif/esptool)

#### Windows only: Use flash download tools

1. [Get it here](https://www.espressif.com/en/support/download/other-tools?5)
2. Erase the flash of the ESP32 befor flashing a new release\
![ESP32Flasher_Erase](https://github.com/VISHNUPRIYANR1812/DRONE-BRIDGE-TELEMTRY-ESP32/assets/134359531/631bf315-e94d-4206-8710-5391ec75358d)

3. Select the firmware, bootloader & partition table and set everything as below
   ```shell
    0x8000 partition_table/partition-table.bin
    0x1000 bootloader/bootloader.bin
    0x10000 db_esp32.bin
    0x110000 www.bin
   ```
   ![ESP32Flasher](https://github.com/VISHNUPRIYANR1812/DRONE-BRIDGE-TELEMTRY-ESP32/assets/134359531/a4ae4b09-5a49-4ed2-89f8-afac1f4c3d82)

3.  Hit Start and power cycle your ESP32 after flashing

### Wiring

1.  Connect the UART of the ESP32 to a 3.3V UART of your flight controller.
2.  Set the flight controller port to the desired protocol.

(Power the ESP32 module with a stable 3.3-5V power source) 
**Check out manufacturer datasheet! Only some modules can take more than 3.3V. Follow the recommendations by the ESP32 boards manufacturer for powering the device**

Defaults: UART2 (RX2, TX2 on GPIO 16, 17)


![Pixhawk_wiring](https://github.com/VISHNUPRIYANR1812/DRONE-BRIDGE-TELEMTRY-ESP32/assets/134359531/ce21613b-e759-40d0-87ad-d63243301388)


### Configuration
1.  Connect to the wifi `DroneBridge ESP32` with password `dronebridge`
2.  In your browser type: `dronebridge.local` (Chrome: `http://dronebridge.local`) or `192.168.2.1` into the address bar.
 **You might need to disable the cellular connection to force the browser to use the wifi connection**
3.  Configure as you please and hit `save`

![dbesp32_webinterface](https://github.com/VISHNUPRIYANR1812/DRONE-BRIDGE-TELEMTRY-ESP32/assets/134359531/57236526-67f3-44b9-a458-029c8533b15c)

**Configuration Options:**
-   `Wifi SSID`: Up to 31 character long
-   `Wifi password`: Up to 63 character long
-   `UART baud rate`: Same as you configured on your flight controller
-   `GPIO TX PIN Number` & `GPIO RX PIN Number`: The pins you want to use for TX & RX (UART). See pin out of manufacturer of your ESP32 device **Flight controller UART must be 3.3V or use an inverter.**
-   `UART serial protocol`: MultiWii based or MAVLink based - configures the parser
-   `Transparent packet size`: Only used with 'serial protocol' set to transparent. Length of UDP packets
-   `LTM frames per packet`: Buffer the specified number of packets and send them at once in one packet
-   `Gateway IP address`: IPv4 address you want the ESP32 access point to have

Most options require a restart/reset of ESP32 module

## Use with DroneBridge for Android or QGroundControl

![dp_app-map-2017-10-29-kleiner](https://github.com/VISHNUPRIYANR1812/DRONE-BRIDGE-TELEMTRY-ESP32/assets/134359531/4c341ae9-d3ea-458f-937e-828d3517b8cc)

-   Use the Android app to display live telemetry data. Mission planning capabilities for MAVLink will follow.
-   The ESP will auto broadcast messages to all connected devices via UDP to port 14550. QGroundControl should auto connect
-   Connect via **TCP on port 5760** or **UDP on port 14550** to the ESP32 to send & receive data with a GCS of your choice. **In case of a UDP connection the GCS must send at least one packet (e.g. MAVLink heart beat etc.) to the UDP port of the ESP32 to register as an end point.**


