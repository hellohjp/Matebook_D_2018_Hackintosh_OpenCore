# Matebook_D_Hackintosh_OpenCore

Language:   
English | [简体中文](https://github.com/Zero-zer0/Huawei_Matebook_D_Hackintosh_OpenCore/blob/master/ReadmeCN.md)
## Huawei Matebook D 2018 (I7 8550U)
### Also works on Magicbook 2018 Intel Edition!

| Specifications | Details                                                                                                            |
|:--------------:|:------------------------------------------------------------------------------------------------------------------:|
| Graphics Cards | Intel UHD620 / nVidia MX150                                                                                        |
| Memory         | 8GB*2 DDR4 2400                                                                                                    |
| Keyboard       | PS2 Keyboard without backlight🙃                                                                                   |
| Trackpad       | ELAN2202 (Requires _OSI Patch to enable GPIO interrupt on macOS)                                                             |
| Sound Card     | ALC256                                                                                                             |
| Screen         | 15.6‘ 1920x1080                                                                                                    |
| SSD            | Hikivision C2000 512GB + LITE-ON 128GB SATA m.2                                                                    |
| WiFi+BT        | Broadcom BCM94360CS2                                                                                         |
| Processor      | Intel Core i5 8250U / i7 8550U  |
| BIOS |   1.30 <br>Secure Boot: `OFF`;  Hardware P-Status(HWP): `ON`    |
---------

## Current Status

### **Basic knowledge on OpenCore is needed!**

* Based on OpenCore Official Release 0.5.3

* Native NVRAM works.

* Soundcard with injected Layout-ID 21 works well.

* ~~CFG Lock cannot be unlocked in Huawei's fxxking BIOS. However everything just works. I'm not able to modify the confusing InsydeH2O BIOS now.~~
    The way to modify BIOS is found, I'll update later to explain how to unlock CFG and change DVMT.

* Replaced stock Intel Wireless 8265AC with BCM94360CS2 and an adapter. Using [AirportBrcmFixup](https://github.com/acidanthera/AirportBrcmFixup) with `brcmfx-country=#a` to enable all WiFi channels.

* Use [one-key-cpufriend](https://github.com/stevezhengshiqi/one-key-cpufriend) to gain a better CPU power management. However, a CpuFridendDataProvider generated by yourself is needed if you have an i5-8250U version of Matebook D 2018 or Magicbook 2018 Intel edition.)

* DGPU nVidia MX150 is disabled by SSDT-DDGPU.

* **Use `USBInjectall.kext` to avoid potential USB map differences. However, customizing your own `USBports.kext` by [Hackintool](http://headsoft.com.au/download/mac/Hackintool.zip) is STRONGLY RECOMMENDED. You can read the help document inside for more details.**


* This EFI may also works on Magicbook 14 Kabylake-R. However, Layout-ID may need to set as `56` or `57` ( May need more modifications)

* You may need to use your own System-UUID when you need dual boot with Windows 10.  

* Minor bugs: 
    1. ~~Screen backlight requires a second-open to turn on after closing lid.~~    
    Now fixed by `SSDT-LID-Wake-After-Sleep` ,thanks to [hjmmc](https://github.com/hjmmc) : [Honor-Magicbook](https://github.com/hjmmc/Honor-Magicbook)    
    I added an `_OSI`  determination to meet the needs of dual boot with Windows/Linux by OpenCore though it's not recommended.  

    2. Trackpad doesn't work and a USB mouse is needed when installing.  
* If you have an NVMe ssd and want to have a better battery life, have a try on NVMeFix.kext.

------
## Something related to `ACPI Errors`
This hackintosh EFI may have some minor ACPI problems related to `Dynamic OEM Tables`, which is a set of SSDTs generated by firmware dynamically in terms of current power status of your computer. These tables transfer current power status to the system by `_PPC`, which will also called by `_QA6` under `EC0`, are related to CPU power management.   

It‘s weird that these tables cannot loaded by OpenCore automatically. What's worse, macOS may unbootale while stucking at APCI Errors related to `_QA6` and `_PPC`.    

A solution to this problem is making these tables force loaded by bootloader. 
1. Boot your Matebook D or Magicbook 2018 with `Clover`,
2. Press `F4` to dump your `ACPI Tables`.
3. Find all of Dynamic SSDTs, which has an `x` ahead of serial number. <br>For example, `SSDT-x3_0-Cpu0Ist.aml` ,`SSDT-x3_1-ApIst.aml`......
4. Put all these `Dynamic SSDTs` into EFI/OC/ACPI
5. "ADD" these SSDTs into OpenCore under `config.plist--ACPI--Add`, just refer to the form of other SSDTs' "registeration". 
6. Reboot and check whether you got a **Zero** ACPI Error boot log lol ! 
---------

## TODOs

* ~~Chinese version of this document(简体中文文档会有的，不过现在用翻译器也能看，I'm just lazy)~~

* ~~Improvements on Hibernation.~~(**The Battery SUCKS!**)

* BIOS unlock tutorial.

* ~~Windows 10 startup item in OpenCore (I think using boot menu is a better choice)~~  <br>

    Do **NOT** use OpenCore to boot windows though it won't cause problems theoretically. Using Boot Menu is recommended.

------

## Credits &  Guides

1. [Acidanthera](https://github.com/acidanthera) for OpenCore (and related documents) , Lilu, AppleALC and other awesome projects.

2. [OC-little](https://github.com/daliansky/OC-little) for their handy ACPI Hotpatches for OpenCore.

3. [Justin](https://github.com/cattyhouse) for  [oc-guide](https://github.com/cattyhouse/oc-guide) 

4. [xjn](https://github.com/xjn819) for [《使用OpenCore引导黑苹果》](https://blog.xjn819.com/?p=543)

5. [Daliansky](https://github.com/daliansky), [bat.bat](https://github.com/williambj1) for [《精解OpenCore》](https://blog.daliansky.net/OpenCore-BootLoader.html)

6. [Steve Zheng](https://github.com/stevezhengshiqi) for [one-key-cpufriend](https://github.com/stevezhengshiqi/one-key-cpufriend) and [XiaoMi-Pro-Hackintosh](https://github.com/daliansky/XiaoMi-Pro-Hackintosh)

7. The Clover EFI I have used by [MOJUNSHOU](https://github.com/MOJUNSHOU) : [MateBooK-D](https://github.com/MOJUNSHOU/MateBooK-D)

8. The LID and Brightness-Key fix by [hjmmc](https://github.com/hjmmc) : [Honor-Magicbook](https://github.com/hjmmc/Honor-Magicbook)

-----
## Screenshot & How I put BCM94360CS2 into the laptop

![  ](https://github.com/Zero-zer0/Matebook_D_Hackintosh_OpenCore/blob/master/截屏2019-12-22下午10.28.37.png)

![   ](https://github.com/Zero-zer0/Matebook_D_Hackintosh_OpenCore/blob/master/BCM94360CS2.jpg)



