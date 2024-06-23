# ESP32 Bluetooth
A simple project aimed at testing the esp32 and creating a basic development enivronment

## Description
Im very interested in the ESP32 devices to the point where I'd like to start evaluating
what a development environment would look like them for me. Goals for this project is
to create a dev environment within vscode using docker and then provide documentation to
make it usable.

## Getting Started
This guide assumes that you have a working installation of:
- WSL2
- Docker

Note that the test board I have uses a CH340 USB <> Serial adapter that WSL2's kernel
doesn't seem to support.  I have tested building but cannot verify that flashing is
working.

### Install usbipd
We need to do one small step within Windows so that we can forward along USB devices
into WSL2 and then later into the dev container.

Launch powershell and run the following command to install usbipd

```
winget install usbipd
```

Once installed the following steps are required in order to allow the devices into
WSL2:
1. Identify the USB device we want to share
1. Bind the device
1. Attach the device

To list the device use the following command
```
usbipd list
```
You'll get an output similar to this:
```
Connected:
BUSID  VID:PID    DEVICE                                                        STATE
1-7    1a86:7523  USB-SERIAL CH340 (COM3)                                       Shared
2-1    046d:0a49  Logitech H820e, USB Input Device                              Not shared
2-3    1462:7b85  USB Input Device                                              Not shared
2-4    8087:0025  Intel(R) Wireless Bluetooth(R)                                Not shared
```
In my system the device I want to share is USB-SERIAL CH340, BUSID 1-7, I'll note down '1-7'

To bind the device use the following command
```
usbipd bind --busid <BUSID>
usbipd bind --busid 1-7
```

Finally, attach it to WSL
```
usbipd attach --wsl --busid 1-7
```

Inside WSL2 you can use
```
dmesg | tail
```

To output kernel messages and ensure that your device is properly connected.  I need a better example
cause mine is found but no driver is loaded:
```
[20374.480436] vhci_hcd vhci_hcd.0: pdev(0) rhport(0) sockfd(3)
[20374.480801] vhci_hcd vhci_hcd.0: devid(65543) speed(2) speed_str(full-speed)
[20374.481145] vhci_hcd vhci_hcd.0: Device attached
[20374.755953] vhci_hcd: vhci_device speed not set
[20374.825969] usb 1-1: new full-speed USB device number 6 using vhci_hcd
[20374.905971] vhci_hcd: vhci_device speed not set
[20374.985981] usb 1-1: SetAddress Request (6) to port 0
[20375.038608] usb 1-1: New USB device found, idVendor=1a86, idProduct=7523, bcdDevice= 2.64
[20375.039029] usb 1-1: New USB device strings: Mfr=0, Product=2, SerialNumber=0
[20375.039350] usb 1-1: Product: USB Serial
```

## Open VSCode in WSL

## Clone / Open the project from within WSL

## Build

## Authors
Roque Obusan - [Email](mailto://roque@obusan.me)
