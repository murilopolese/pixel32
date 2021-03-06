# Pixel32 - Code MicroPython on your Pixel Kit on the browser!

![](https://i.imgur.com/RbWlvMw.png)

Check [more the screenshots here](https://imgur.com/gallery/EjdN1ls)

Pixel32 is a MicroPython application running on Kano Pixel Kit that allows it to be programmed on the browser.

## Installing Pixel32: The easy way

Download [Pixel Kit Flash Tool](https://github.com/murilopolese/kano-pixel-kit-flash-tool) and flash MicroPython on it (it will come with Pixel32).

## Installing Pixel32: The hard way

Check [Building](#building) below.

## Basic usage

The first time your Pixel Kit boots it will always display the blue screen. At this point, Pixel Kit has created its own wifi network and it should be named something like PIXEL_KIT_XXXX but with a number instead of the XXXX.

When you connect to this wifi network ~~you can either wait for the Captive Portal page to pop up or~~ go to your browser, type http://192.168.4.1 and start exploring.

The Pixel Kit ip address while displaying the blue screen will never change: it will always be `192.168.4.1`. Learn more about the blue screen on the [documentation](www/docs.md#boot-screens).

**TIP**: Loading the terminal page and docs from the board can be slow. It's highly recommended that you save this pages on your computer for a much better experience. Both terminal and documentation can be saved on your computer by clicking on the download button you will find on the terminal page. You can run them locally by simply double clicking the `html` file you saved.

## Features

- Includes `microWebSrv`, `MicroDNSSrv`, `PixelKit` and `PixelTurtle` libraries. Check [python folder](python) to read the source code.
- WebREPL enabled.
- Boot sequence that can either connect to a wifi or start it's own wifi network.
- Once connected, displays the board's ip address in binary format on the screen.
- Host a html page that allow you to connect and code your Pixel Kit on the browser.
- You can connect to the Pixel Kit by typing the ip address in decimal or binary format on the html page hosted by the board.
- Host full documentation on how to connect, use `PixelKit` and `PixelTurtle` libraries and a very quick Python introduction. This documentation is hosted by the board on `http://IPADDRESS/index.html/#!/docs` and on [GitHub](www/docs.md).
- Both terminal and documentation can be saved on your computer by clicking on save button on the interface. You can run them locally by simply double clicking the `html` file you saved.
- _When connected to the wifi network created by the Pixel Kit, it starts a captive portal: Once you connect to this wifi it will pop up the terminal for you automagically. (**Disabled while interface isn't responsive enough**)_

## Known problems/bugs:

- Documentation anchor links overrides link (refresh will cause to show the terminal page)

## Building it yourself

Use the [Docker image](https://github.com/murilopolese/kano-pixel-kit-pixel32-docker-build) to build the front end and create a FAT partition to flash into your Pixel Kit.

Otherwise there are 2 steps for the build.

### Building the front end

1. Run yarn install on www folder.
1. Run yarn run build on www folder.

### Creating the FAT partition

Check the [Dockerfile](https://github.com/murilopolese/kano-pixel-kit-pixel32-docker-build/blob/master/Dockerfile) for what software you should have installed and the [build script](https://github.com/murilopolese/kano-pixel-kit-pixel32-docker-build/blob/master/scripts/build.sh) to see how to create, format and transfer files to a FAT partition.

### Flashing the Pixel Kit

Ideally you would use [Pixel Kit Flash Tool](https://github.com/murilopolese/kano-pixel-kit-flash-tool) but if you are building yourself you are either a brave person, don't have a supported OS or want to modify the source code. In any of those cases you will need to:

Install [`esptool`](https://github.com/espressif/esptool):

```bash
pip install esptool
```

Download a [MicroPython firmware for ESP32](https://micropython.org/download#esp32).

And finally run `esptool` to flash the MicroPython firmware on address `0x1000` and the Pixel32 FAT partition on the address `0x200000` (those addresses are very important).

```bash
esptool -p /dev/your.pixel.kit write_flash 0x1000 micropython-firmware.bin 0x200000 pixel32partition.img
```
