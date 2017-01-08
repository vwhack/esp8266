# Porting micropython to the esp8266

**Instructions for porting micropython to the ESP-12 model of the esp8266 micro controller**

## Hardware used

[esp8266 wifi serial dev kit development board](https://www.google.co.uk/search?q=esp8266+wifi+serial+dev+kit+development+board) This is a good dev board and includes quite a few GPIO pins to get you going
<img src="http://img.dxcdn.com/productimages/sku_381839_3.jpg" width="200" height="200">


[FT232RL FTDI USB to TTL Serial Adapter Module](http://www.gearbest.com/boards-shields/pp_415190.html) This is the simplest way to erase and flash the esp8266

<img src="http://gloimg.gearbest.com/gb/pdm-product-pic/Electronic/2016/07/23/goods-img/1482478358369264957.jpg" width="200" height="200">

[Mini breadboard](https://shop.pimoroni.com/products/colourful-mini-breadboard) This will help with breaking out power, ground and GPIO pins

<img src="https://cdn.shopify.com/s/files/1/0174/1800/products/colourfulminibreadboard-white.jpg" width="200" height="200">

[Breadboard wire connectors](http://www.ebay.co.uk/itm/like/262359393133) Make sure you get a mixture of male to male and female to female connectors

<img src="http://image.ec21.com/image/mikiwang/OF0011060019_1/Sell_breadboard_wire.jpg" width="200" height="200">


## Flashing

### Pin connectors

To flash the esp8266 you will first need the awesome [esptool](https://github.com/themadinventor/esptool). The simplest way of installing it is using pip `pip install esptool`

The next step is putting the micro controller into bootloader mode. This is done by tying GPIO0 (pin 12 in image) and GPIO15 (pin 10 in image) to GND (pin 9 in image)

Next ensure you have connected the 3.3v supply FTDI VCC (pin 3), esp8266 VCC (pin 8) & CH_PD (pin 3)

Next connect FTDI TXD (pin 4) to esp8266 RXD (pin 15) and FTDI RXD (pin 5) to esp8266 TXD (pin 16)

<img src="http://www.agcross.com/wp-content/uploads/2015/09/ESP8266-ESP-12-wiring-diagram-required-for-bootloader-mode-for-flashing-585x395.png">

For further information on the pinouts see [www.agcross.com](http://www.agcross.com/2015/09/the-esp8266-wifi-chip-part-3-flashing-custom-firmware/) 

Once the FTDI USB to TTL Serial Adapter is connected to the esp8266 the next task is to erase the existing flash. 

You may need to check what port the Serial Adapter is showing on but in Linux the default is `/dev/ttyUSB0` 

### Erasing the flash

Using the esptool.py type `esptool.py --port /dev/ttyUSB0 erase_flash`


### Deploying the new firmware

Download the latest firmware from [http://micropython.org/download/#esp8266](http://micropython.org/download/#esp8266) 

Using the esptool.py type `esptool.py --port /dev/ttyUSB0 --baud 460800 write_flash --flash_size=detect 0 esp8266-2016-05-03-v1.8.bin`

## Installing the MU editor
Thanks for the great work of [Nicholas Tollervey](https://twitter.com/ntoll) who created the original MU editor for the [BBC micro:bit](https://www.microbit.co.uk) there is now a forked branch specifcally developed for the esp8266.

**To install the editor:**
* Clone the repository `clone https://github.com/eduvik/mu.git` 
* Switch to the feature/multiboard branch `git checkout origin/feature/multi-board` 
* Create a virtual environment `python3 venv -m myvenv` and activate it `source myvenv/bin/activate`
* Install the dependant Python packages `pip install -r requirements.txt`

To run the editor type `python3 run.py`






