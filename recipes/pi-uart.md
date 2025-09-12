# Use Adafruit PiUART to connect to virtual console
The [Adafruit PiUART](https://www.adafruit.com/product/3589) is a USB Console and Power Add-on for Raspberry Pi. Instructions for getting the UART-board up and running can be found [here](https://learn.adafruit.com/adafruit-piuart-usb-console-and-power-add-on-for-raspberry-pi).

## Quickstart
Insert the SD card the reader in your laptop, and add the following entries to the `[all]` section in `config.txt`.

```
[all]
dtparam=uart0=on
dtparam=uart0
dtparam=uart0_console
enable_uart=1
```

Insert the card in the Raspberry Pi and boot the system. You should now be able to connect to a Linux virtual console on the board through a serial connection as described in [these instructions](https://learn.adafruit.com/adafruit-piuart-usb-console-and-power-add-on-for-raspberry-pi/setup-software).
