# ESP32-Radio-TTGO
ESP32 Internet Radio with Display 

## Introduction
Existing radios or loudspeakers will be used in order to get decent sound quality.
The display should show relevant information and the font size should not be too small.

Most of the time you just want to switch the device on and listen to the music.
There are only two things you want to change: volume and radio station.
Nevertheless there is only one dial to cover both functions.

In addition there is a webserver including song covers as well as mqtt to allow integration into home automation.

## Operation
### Display
#### Default view
![Display-Default](README/images/ESP_Radio_Display-3.jpg)

#### Select radio station
![Display-Station](README/images/ESP_Radio_Display-2.jpg) 

#### Change Volume
![Display-Volume](README/images/ESP_Radio_Display-1.jpg)

### Webserver
![webserver](README/images/ESP_Radio_WEB_1.jpg)
### MQTT
![MQTT](README/images/ESP_Radio_MQTT_1.jpg)
### Node-RED
![node-RED](README/images/ESP_Radio_NodeRed_1.jpg)

## Setup
### WiFi Manager
Upon first boot, the wifi  manager will start and ask for wifi credntials as well as mqtt setting.
### Radio Stations
10 stations can be configured using the four parameters below. PlatformIO: data/setup.ini
- RadioName = 1Live
- RadioURL = http://wdr-1live-live.icecast.wdr.de/wdr/1live/live/mp3/128/stream.mp3
- RadioTitleSeperator = -
- RadioTitleFirst = Artist

| Token               | Comment    |
| :----------         | :----- |
| RadioName           | A short name to be shown onthe display |
| RadioURL            | url of the mp3 stream |
| RadioTitleSeperator | Some stations are using '/' or '-' to split artist and title |
| RadioTitleFirst     | Indicate if order is Title/Artist or Artist/Title |


## Hardware

![TTGO-1](README/images/ESP_Radio_1.jpg)

Despite of the TTGO you need the audio decoder and a rotary encoder.

### Digital / Analog Converter – DAC
The PCM5102 is using the I2S interface.

| PCM5102     | ESP    |
| :---------- | :----- |
| P_I2S_LRCK  | Pin 25 |
| P_I2S_BCLK  | Pin 26 |
| P_I2S_DATA  | Pin 22 |


In addition you need to connect the following pins:

| PCM5102 | Wiring      | Comment |
| :------ | :---------- |:----- |
| FLT     | GND         | Filter Select. GND for normal latency|
| DEMP    | GND         | De-Emphasis control. Low = off|
| SMT     | GND         | Audio format. Low = I2S|
| XMT     | 10K -> 3V3  | Mute: pulled via 10k Resistor to 3,3V to un-mute|

### Rotary Encoder

| Encoder     | ESP    |
| :---------- | :----- |
| P_ENC0_A    | Pin 12 |
| P_ENC0_B    | Pin 13 |
| P_ENC0_BTN  | Pin 15 |
| P_ENC0_PWR  | Pin  2 |


## Software 

### WiFi Manager



# PlatformIO

### LittleFS

The build in upload tool for ESP32 does not work. Two files are used to overcome this:
- littlefsbuilder.py
- mklittlefs.exe

#### platformio.ini
board_build.filesystem = littlefs
extra_scripts = ./littlefsbuilder.py

### TFT_eSPI

Either copy the user files files or manual adjust as shown below.
- source: README\TFT_eSPI 
- target: PlatformIO\Projects\TTGO-Radio\.pio\libdeps\esp32doit-devkit-v1\TFT_eSPI

#### User_Setup.h
File: PlatformIO\Projects\TTGO-Radio\.pio\libdeps\esp32doit-devkit-v1\TFT_eSPI\User_Setup.h
<br> Comment the default with “//” and use ST7789 ,i.e. remove the comment signs “//”

- // #define ILI9341_DRIVER
- #define ST7789_DRIVER      // Full configuration option, define additional parameters below for this display

#### User_Setup_Select.h
File: PlatformIO\Projects\TTGO-Radio\.pio\libdeps\esp32doit-devkit-v1\TFT_eSPI\User_Setup_Select.h

- // #include <User_Setup.h>
- #include <User_Setups/Setup25_TTGO_T_Display.h>    // Setup file for ESP32 and TTGO T-Display ST7789V SPI bus TFT
