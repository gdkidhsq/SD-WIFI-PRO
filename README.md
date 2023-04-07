# SD WIFI PRO



![image-20230327135120562](assets/SD_WIFI_PRO_main.png)

## 1. Introduction

This is a brand new SD WIFI, we call it SD WIFI PRO, or SWP for short. It is based on ESP32 PICO V3-02 and SD 7.0 size standard, with additional 8MB Flash, 2MB PSRAM, which can provide more possibilities for more creative applications.
By default, we provide a set of open source and available websever firmware, which has complete file upload and download functions, and can basically meet the needs of remote management files in the LAN.
In addition to the necessary SD transmission pins, SWP also leads out redundant pins. In addition to being used for firmware updates, it can also be used for other device control through the serial port/I2C, so as to realize remote upload files, send control commands and many other operations.
It can also be used in scenarios other than 3D printing, such as Flash Air (TOSHIBA), ez Share (Ez Share) and other products on the market, for file sharing, such as quickly transferring photos in the camera without a data cable to the phone.
It is not only a completely open source product, but also a wireless SD product development kit, which provides more possibilities according to your imagination.

## 2. Feature

- Based on ESP32 PICO V3-02, with additional 8MB Flash, 2MB PSRAM
- 2.4G Wifi & Bluetooth
- SD 7.0 size standard, SDIO/SPI compatible golden finger, and additional serial port and I2C interface
- Built-in 8GB high-speed memory
- Comes with SD Reader and Flash Uploader development board
- Fully open source hardware and software

## 3. Application

3D printers, and other devices that require wireless access to SD cards.


## 4. Hardware Guide

### 4.1 Pin Functions

![image-20230328082527339](assets/SD_WIFI_PRO_pin.png)

| Pin  |       Name        | Input     | Output | Function                                                     |
| ---- | :---------------: | --------- | ------ | ------------------------------------------------------------ |
| 1    |      SDIO_D3      |           |        | Built-in Flash pin, SPI: CS                                  |
| 2    |     SDIO_CMD      |           |        | Built-in Flash pin, SPI:MOSI                                 |
| 3    |        GND        |           |        | Ground                                                       |
| 4    |        3V3        |           |        | 3.3V power supply voltage for SWP（3.0-3.6V @300mA+）        |
| 5    |     SDIO_CLK      |           |        | Built-in Flash pin, SPI: CLK                                 |
| 6    |        GND        |           |        | Ground                                                       |
| 7    |      SDIO_D0      |           |        | Built-in Flash pin, SPI: MISO                                |
| 8    |      SDIO_D1      |           |        | Built-in Flash pin, SPI: NC                                  |
| 9    |      SDIO_D2      |           |        | Built-in Flash pin, SPI:NC                                   |
| 10   |      GPIO_32      | OK        | OK     | ADC1_CH4,<br/>TOUCH9, <br/>RTC_GPIO9                         |
| 11   |      GPIO_26      | Ok        | OK     | GPIO26, DAC_2, ADC2_CH9, RTC_GPIO7, EMAC_RXD1                |
| 12   |      GPIO_2       | OK        | OK     | must be left floating or LOW to enter flashing mode<br/>ADC2_CH2,<br/>TOUCH2,<br/>RTC_GPIO12,<br/>HSPIWP, HS2_DATA0,<br/>SD_DATA0 |
| 13   |      GPIO_0       | pulled up | OK     | outputs PWM signal at boot, must be LOW to enter flashing mode<br>ADC2_CH1,<br/>TOUCH1,<br/>RTC_GPIO11,<br/>CLK_OUT1,<br/>EMAC_TX_CLK |
| 14   |      ESP_EN       |           |        | Enable (EN) is the 3.3V regulator’s enable pin. It’s pulled up, so connect to ground to disable the 3.3V regulator. This means that you can use this pin connected to a pushbutton to restart your ESP32, for example. |
| 15   |   GPIO_3 / RX0    | OK        | RX pin | HIGH at boot                                                 |
| 16   |   GPIO_01 / TX0   | TX pin    | OK     | debug output at boot                                         |
| 17   | GPIO_22 / I2C_SCL | OK        | OK     | GPIO22, VSPIWP, U0RTS, EMAC_TXD1                             |
| 18   | GPIO_21 / I2C_SDA | OK        | OK     | GPIO21, VSPIHD, EMAC_TX_EN                                   |
| 19   |      GPIO_19      | OK        | OK     | GPIO19, VSPIQ, U0CTS, EMAC_TXD0                              |

### 4.2 Uploader & Card Reader Board (SWP Dev Board) Instructions

![image-20230328092433777](assets/SD_WIFI_PRO_Dev_board_main.png)



#### 4.2.1 Function Description

![image-20230328092415667](assets/SD_WIFI_PRO_Dev_board_main_ins.png)

1. SD7.0 21Pin Socket

2. All pins out Of SWP

3. Card reader chip (USB2.0) & USB switch chip

4. USB Type-C Connector

5. Firmware automatic download and serial debugging circuit (CH340)

6. Mode/function selection DIP switch, It is defined as follows:

| Switch pin                                                   | Status | Mode/Function                                                |
| ------------------------------------------------------------ | ------ | ------------------------------------------------------------ |
| 1 <br>The control pin of the built-in *Flash interface switching chip*,it is connected to GPIO_26 of ESP32,  pull-up resistor. By default, Flash interface is connected to the golden finger of the SD shell. | ON     | Built-in 8GB Flash is connected to ESP32.                    |
| 1                                                            | OFF    | Built-in 8GB Flash is connected to the golden finger of the SD shell. |
| 2<br>The control pin of the USB switching chip, pull-down resistor, USB is connected to the card reader chip by default. | ON     | USB connect to CH340 for firmware download and debugging     |
| 2                                                            | OFF    | USB connect to Card reader for reading built-in 8GB flash.   |

#### 4.2.2 USE the dev board as a SD socket

For motherboards without SD card sockets, SWP may not be convenient to connect. At this time, you can use the development board as a socket, and only need to connect it to the original SPI interface of the motherboard.
For some boards that only have a TF card holder and do not have an SPI interface reserved, it will be more troublesome, but there are many cheap TF to SD card boards available for sale.

## 5. Firmware Guide

### 5.1 Access SWP by web using AP firstly
Insert SWP into the SWP Dev Board, after power up, SWP will create a wireless access points, the AP ssid named is "SD-WIFI-PRO-RD".
When connect wirelessly to SD-WIFI-PRO-RD, after success, you can access the SWD by web browser.
The default AP local ip is 192.168.4.1, so usging "http://192.168.4.1" the Web address to asscess to SWP.

### 5.2 Web functions
Assuming that in the AP connected state.
1. List files
    Usging "http://192.168.4.1", to list all the files in the SD card of SWD(8GB high-spped memory) .
    Click "List Files" at navigation bar will show the page when in other pages.
2. Upload files to SWP
    In index.htm page, at the "Update File To Server" block, select a file , click "Upload" button.
    Click "List Files" at navigation bar will show the page when in other pages.
3. Browse image
    Brownse only images in imglist.htm page. 
    Click "List Images" at navigation bar will show the page when in other pages.
4. Setting
    Click "WiFi Setting" at navigation bar will show the page when in other pages.
    In wifi.htm page, at "Set Station Parameters" block, input the right network ssid and password ,can let the SWD connect to a WiFi network.
    After do "Connect To Station", if sucessful, at "Status" block ,can find the SWD local ip, using the local ip can access SWD When SWD and your access device(such as PC) are in the above network.
    In wifi.htm page, at "Set To AP mode" block, can change SWD into AP mode.

### 5.3 Know the current network status of SWP
Assuming that in the AP connected state.
1. Check NW_STATUS.INI file
    a)When access http://192.168.4.1/, the browse show a "NW_STATUS.INI" file, download the file and look at the content to check the network status.
    b)There is other way to check the NW_STATUS.INI. Put the SWD with SD card sockets or adapter into PC, can read find NW_STATUS.INI at U disk.
3. By wifi.htm web page
    Access http://192.168.4.1/wifi.htm, the status output box can show the network status.
3. By uart serial command
    Use M53 serail command.
    
 There are three way to check network status, and there three way to config network too. To config network, the first way is using wifi.htm page, the second is using serial command, the third is using SETUP.INI file at SD card of SWD.  

### 5.4 Using serial to get the runtime information and Config SWD network  
1. Get the runtime information 
    Insert SWD into SWP dev board socket, let USB cable inset to USB Type-C connector, and plug the cable inser to PC USB port.
    Let the DIP is at the following status: switch 1 is at OFF status, switch 2 is at ON status. 
    If SWD and dev board are OK, "USB-SERIASL CH340(COMX)" will show at Ports COM & LPT in Device Manager in Windows 10.
    Use serial terminal to connect to SWD, the options are "115200 Baud rate, 8 data bits, 0 parity bits, 1 stop bit, No flow control".
    If the above is OK , now you can see the information when you access web page in SWD.

2. Config SWD network by serial command
    the uart command usage as the following:
    1)To AP Mode:
      M49: Set the wifi AP mode , 'M49 AP' 
      M52: Start to AP mode, 'M52'
    2)To STA Mode:  
      M49: Set wifi STA mode , 'M49 STA'
      M50: Set wifi ssid , 'M50 ssid-name'
      M51: Set wifi password , 'M51 password'
      M52: Start to connect the wifi, 'M52'
    3)To Show WiFi Status:
      M53: Check wifi connection status, 'M53' 

### 5.5 Config SWD netwok by SETUP.INI file as SD
  Put SETUP.INI file in the root of the in the SD card of SWD(8GB high-spped memory) .
  1.Config STA, input the following example content into SETUP.INI, must change the SSID and PASSWORD key content as yours.
    WIFIMODE=STA
    SSID=FACTORY-TEST
    PASSWORD=?Umv870q
  2.Config AP, input the following example content into SETUP.INI 
    WIFIMODE=AP    

### 5.6 Compile firmware
  See the readme of the SWD firmware source code.

## 6. Part List

- SD WIFI PRO x1
- Uploader & Card Reader Board x1

## 7. Documentations

[ESP32 PICO V3-02 Chip Datasheet EN](https://www.espressif.com/sites/default/files/documentation/esp32-pico-v3-02_datasheet_en.pdf) <br/>
[ESP32 PICO V3-02 Chip Datasheet CN](https://www.espressif.com/sites/default/files/documentation/esp32-pico-v3-02_datasheet_cn.pdf) <br/>
[Uploader & Card Reader Board Schematic](https://github.com/FYSETC/SD-WIFI-PRO/blob/main/SD-WIFI-PRO%20Uploader.pdf) <br/>
[SD WIFI PRO Schematic](https://github.com/FYSETC/SD-WIFI-PRO/blob/main/SD-WIFI-PRO%20V1.0.pdf) <br/>
[Firmware](https://github.com/FYSETC/SdWiFiBrowser)

## 8. Where to buy

<a href="https://www.aliexpress.us/item/3256805122957973.html"><img src="images/Aliexpress.png" alt="aliexpress" height="80" style="position:relative;left:200px;" /></a><a href=" "><img src="images/taobao.png" alt="taobao" height="80" style="position:relative;left:600px;" /></a> <a href=""><img src="images/fysetc_online_shop.png" alt="fysetc" height="80" style="position:relative;left:1000px;"/></a> 


## 9. Tech Support

Please submit any technical issue into our [forum](http://forum.fysetc.com/) ，github，facebook, Discord。
