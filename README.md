## 96Boards Mezzanine LCD Kit
[Availble here](http://www.lenovator.com/product/102.html), it can be used on any 96Boards CE products to let us use the LCD display.

### Package Include
- 1 PCS MIPI LCD with resolution 1280*800, landscape view
- 1 PCS mezzanie board to bridge the MIPI LCD and 96Boards
- 1 PCS FPC cable

**Note:** there is **only a display panel**, there is **not** touch screen on the display panel.

### Specification
- Mezzanie board schematic: download [here](https://github.com/Tony-HIT/96Boards_Mezzanine_LCD_Kit/tree/master/specification)
- MIPI LCD display datesheet: download [here](https://github.com/Tony-HIT/96Boards_Mezzanine_LCD_Kit/blob/master/specification/HC070IY35023-16(800x1280)_Product_Spec.pdf)


### Support List
Now this LCD Kit has been tested on the 96Boards below:
- HiKey(LeMaker version): Debian. (AOSP may support, but I do not have the source or image)
- DrangonBoard 410c: Debian
- Bubblegum-96: Still under test

### Connection
We need pay attention to the FFC cable connection between LCD and adapter board, we can refer to the pictrue below.
![cable_connection](https://github.com/Tony-HIT/96Boards_Mezzanine_LCD_Kit/blob/master/img/cable_connection.png)

### Quick Start
#### HiKey(LeMaker version) Deabian for EMMC Download
##### Linaro Reference Platform Build (Suggested)
At 12/26/2016 this day, I found Linaro Reference Platform Build already support the mipi lcd. We can downlod the image from [here](http://github.com/Linaro/documentation/tree/master/Reference-Platform/Releases/RPB_16.06).
We should get the files below:
- hisi-idt.py
- l-loader.bin
- ptable-linux-8g.img
- hikey-boot-linux-20160629-120.uefi.img
- hikey-debian-jessie-alip-sdcard-20160629-120.img
In the RPB1606, the mipi panle driver is compiled into kernel modules, so we need load the driver at the boot time. We can add the words below into /etc/modules:
```
panel-hikey
```

##### Customizd Debian 
We can also download the pre-built kernel and Debian image from [here](http://www.lemaker.org/product-hikey-download-45.html). We need get the files below:
- hisi-idt.py
- l-loader.bin
- ptable-linux-8g.img
- boot-fat.uefi
- jessie.updated.img

#### HiKey(LeMaker version) Deabian for EMMC Installation
After downloading, we need close the J601 CONFIG header "1 and 2 pins", "3 and 4 pins", open "5 and 6 pins" so that we can update the software in the HiKey Emmc. Connect the HiKey to the host Linux PC (the host Linux PC should have the [fastboot environment](https://github.com/96boards/documentation/blob/master/ConsumerEdition/HiKey/Installation/BoardRecovery.md#make-sure-fastboot-is-set-up-on-host-computer/)) via the micro-usb on the HiKey, and then power on the HiKey.

Wait about 5 seconds and then check that the HiKey board has been recognized by your Linux PC:
```
$ ls /dev/ttyUSB*
```
Make sure the modem interface is in the right ttyUSB as previously suggested. In this example, use ttyUSB0:

```
$ sudo python hisi-idt.py -d /dev/ttyUSB0 --img1=l-loader.bin
```
After the python command has been issued you should see the following output. 

```
+----------------------+
 Serial:  /dev/ttyUSB0
 Image1:  l-loader.bin
 Image2:  
+----------------------+

Sending l-loader.bin ...
Done
```
**NOTE:** You may see the word “failed” before Done. This is under investigation but is not fatal. As long as the “Done” is printed at the end you may proceed.

Then we can use fastboot command to flash the boot, kernel and system:
```
$ sudo fastboot flash ptable <ptable_FILE_NAME>
$ sudo fastboot flash boot <uefi_boot_FILE_NAME>
$ sudo fastboot flash system <debian_rtfs_FILE_NAME>
```
After all the files have been flashed into the EMMC, we need remove link "3 and 4" from J601 CONFIG  header. Connect the mezzanine board to the HiKey, connect the MIPI LCD to the mezzanine board, **disconnect the HDMI out**, then power on the HiKey board. You should see the system windows on the MIPI LCD.

**Note:** if the HiKey Emmc has been swapped without anything, you need first do the [BoardRecovery](https://github.com/96boards/documentation/blob/master/ConsumerEdition/HiKey/Installation/BoardRecovery.md) to install the necessary boot files. Then continue to update the software to support the MIPI LCD.

### Kernel Source Code 
#### Linaro Reference Platform Sourc Code (Suggested)
Please refer here to download the RPB 1606 kernel source code:[here](https://github.com/96boards/linux/tree/96b/releases/2016.06)
Building suggestion: [here](https://github.com/Linaro/documentation/blob/master/Reference-Platform/Releases/RPB_16.06/ConsumerEdition/HiKey/BFSDebianRPB.md)

#### HiKey Kernel 4.5 Source Code
Please refer to [HERE](https://github.com/xin3liang/linux/tree/hikey-tracking-integration-devel-drm-dsi-panel) for the MIPI LCD source code. The latest four commit is for the MIPI LCD.










