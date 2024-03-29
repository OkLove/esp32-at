- [ESP32 platform](#platform-esp32)  
- [ESP8266 platform](#platform-esp8266)  

More supported modules, please referred to [`components/customized_partitions/raw_data/factory_param/factory_param_data.csv`](components/customized_partitions/raw_data/factory_param/factory_param_data.csv)

<a name="platform-esp32"></a>
# ESP32 platform  

## Hardware Introduction
The WROOM32 Board sends AT commands through UART1 by default. 

* GPIO16 is RXD
* GPIO17 is TXD
* GPIO14 is RTS
* GPIO15 is CTS

The debug log will output through UART0 by default, which TXD0 is GPIO1 and RXD0 is GPIO3, but user can change it in menuconfig if needed.  

* `make menuconfig` --> `Component config` --> `ESP32-specific` --> `UART for console output`

### Notes: Please pay attention to conflict of the pin ##
- If choose `AT through HSPI`, you can get the information of the hspi pin by `make menuconfig` --> `Component config` --> `AT` --> `AT hspi settings`
- If enable `AT ethernet support`, you can get the information of the ethernet pin from `ESP32_AT_Ethernet.md`.

## Compiling and flashing the project
Compiling the esp32-at is the same as compiling any other project based on the ESP-IDF:

1. You can clone the project into an empty directory using command:
```
git clone --recursive https://github.com/espressif/esp32-at.git
```
2. `rm sdkconfig` to remove the old configuration and `rm -rf esp-idf` to remove the old esp-idf if you want to compile other esp platform AT.
3. Set the latest default configuration by `make defconfig`. 
4. `make menuconfig` -> `Serial flasher config` to configure the serial port for downloading.
5. `make flash` to compile the project and download it into the flash.
  * Or you can call `make` to compile it, and follow the printed instructions to download the bin files into flash by yourself.
  * `make print_flash_cmd` can be used to print the addresses of downloading.
  * More details are in the [esp-idf README](https://github.com/espressif/esp-idf/blob/master/README.md).
6. `make factory_bin` to combine factory bin, by default, the factory bin is 4MB flash size, DIO flash mode and 40MHz flash speed. If you want use this command, you must fisrt run `make print_flash_cmd | tail -n 1 > build/download.config` to generate `build/download.config`.
7. If the ESP32-AT bin fails to boot, and prints "ota data partition invalid", you should run `make erase_flash` to erase the entire flash.
8. Since we updated the toolchain recently, it is not compatible with the old version. Please use the toolchain we provided in the  `esp32-at/esp-idf/docs/get-started/linux-setup.rst` and `esp32-at/esp-idf/docs/get-started/windows-setup.rst` to build your ESP32 AT project.

<a name="platform-esp8266"></a>
# ESP8266 platform  

## Hardware Introduction
The ESP8266 WROOM 02 Board sends AT commands through UART0 by default. 

* GPIO13 is RXD
* GPIO15 is TXD
* GPIO1  is RTS
* GPIO3  is CTS

The debug log will output through UART1 by default, which TXD0 is GPIO2, but user can change it in menuconfig if needed.  

* `make menuconfig` --> `Component config` --> `ESP8266-specific` --> `UART for console output`


## Compiling and flashing the project
Compiling the ESP8266 AT is the same as compiling esp32-at:

1. You can clone the project into an empty directory using command:
```
git clone --recursive https://github.com/espressif/esp32-at.git
```
2. Change the Makefile from  
```  
export ESP_AT_PROJECT_PLATFORM ?= PLATFORM_ESP32
export ESP_AT_MODULE_NAME ?= WROOM-32
```    
to be   
```  
export ESP_AT_PROJECT_PLATFORM ?= PLATFORM_ESP8266 
export ESP_AT_MODULE_NAME ?= WROOM-02
```  
3. `rm sdkconfig` to remove the old configuration and `rm -rf esp-idf` to remove the old esp-idf if you want to compile other esp platform AT.
4. Set the latest default configuration by `make defconfig`. 
5. `make menuconfig` -> `Serial flasher config` to configure the serial port for downloading.
6. `make flash` to compile the project and download it into the flash.
  * Or you can call `make` to compile it, and follow the printed instructions to download the bin files into flash by yourself.
  * `make print_flash_cmd` can be used to print the addresses of downloading.
  * More details are in the [esp-idf README](https://github.com/espressif/esp-idf/blob/master/README.md).
7. `make factory_bin` to combine factory bin, by default, the factory bin is 4MB flash size, DIO flash mode and 40MHz flash speed. If you want use this command, you must fisrt run `make print_flash_cmd | tail -n 1 > build/download.config` to generate `build/download.config`.
8. If the ESP32-AT bin fails to boot, and prints "ota data partition invalid", you should run `make erase_flash` to erase the entire flash.
9. Since we updated the toolchain recently, it is not compatible with the old version. Please use the toolchain we provided in the  `esp32-at/esp-idf/docs/get-started/linux-setup.rst` and `esp32-at/esp-idf/docs/get-started/windows-setup.rst` to build your ESP32 AT project.