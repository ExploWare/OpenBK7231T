
## OpenBK7231T is an open source firmware for BK7231T-based Tuya modules, including WB2S, WB3S and much more
Our goal is to provide Tasmota-like system for BK7231T smart devices.

For compiling and generic SDK information see here:
https://www.elektroda.com/rtvforum/topic3850712.html

To support project, send a donation:
https://paypal.me/openshwprojects

If you have a device and want to get it support, send me message in:
https://www.elektroda.com/rtvforum/topic3850712.html

## Installation
Installation instruction is here, in post #10 (2022.01.28): https://www.elektroda.com/rtvforum/viewtopic.php?p=19849072#19849072
---
### /// Work In Progress /// My adopted interpretation of the intallation process ///
---

THe files in releases/ are the files from the original author. No warranties is blame 

WB2S will only require two UART connections. So you will need the pins:
- RX and TX (for programming)
- 2RX and 2TX (OPTIONAL for checking the output log on the UART from the chip)
- of course, the GND and power supply:
  - 5volt can be supplied to the LDO. Original author supplied from USB and removed the large electrolytic capacitor in front of the LDO to prevent the USB from consuming too much current at startup, which results in a USB device reset and problems with WB2S programming!
  - I suplied the 5volt from an external 5 volt power supply and introduced a switch on that line. Recent thoughts advice to connect the CEN (Chip ENable, and pull that to ground for a reset, but that would demand a fiddly solder job on my boards.

## My victims:
![qiachip smart switch](https://user-images.githubusercontent.com/6767397/152825755-6c450545-ff34-43aa-9124-935779ae714c.png)
![WB2S Front](https://user-images.githubusercontent.com/6767397/152825975-6af0712b-2e06-4532-bed3-e2d1b1da3947.png)
![WB2S Back](https://user-images.githubusercontent.com/6767397/152826015-82cf6236-4a0c-4fdd-8883-b8a894a17374.png)

![plug](https://user-images.githubusercontent.com/6767397/153162414-43a18ade-9ad6-444a-a8c4-03ad4f3d845d.png)
![plug details](https://user-images.githubusercontent.com/6767397/153162497-96930a32-94a8-4544-b2ac-4f4846c0cb3b.png)

This one has the ability to report power consumtion, via the BL0937 chip, wired to PWM1=CF PWM2=CF1 and PWM4 = SEL, thats a future feature
https://www.belling.com.cn/media/file_object/bel_product/BL0937/datasheet/BL0937_V1.02_en.pdf




![Design of the WB2S module](https://user-images.githubusercontent.com/6767397/151781776-97e7dc71-fbcb-4d42-b0f7-7a8fc3319ff6.png)

![connections](https://user-images.githubusercontent.com/6767397/152822926-41781fbe-1e00-4973-b3da-5c6511e2badf.png)
I soldered the TX, RX and GND to the board and the 5V onto the positive pin of the capacitor


I failed WRITING with the FTDI UMFT234XD module, being not able to select a working baudrate. It did 'support' 921600 but failed.

Connecting this to a USB to TTL CH340G style dongle worked with 921600

![FTDI UMFT234XD](https://user-images.githubusercontent.com/6767397/152821758-23f5f7d4-5a5b-44c3-b777-5c49d408259c.png)
![CH340G](https://user-images.githubusercontent.com/6767397/152821715-2b453591-29df-4407-8ddb-4a7cfaab7f44.png)


###OPTIONAL###
![Image of the 2TX/2RX wiring](https://user-images.githubusercontent.com/6767397/151781537-1a95a645-9119-4a75-bc07-e330caf6d90f.png)
This is just 'nice' and handy when things are not working as they are supposed to. It also shows your WiFi credentials in plain text. Unsafe when in public acccess, useful when debugging.
I created a pressure fit connection for these. Took some fiddling to get connected right, but reduced a lot of awkard solder work
![bracket](https://user-images.githubusercontent.com/6767397/152821307-6f30a367-438f-4936-9104-bed8f7f9f961.png)



![Image of the Reset switch and blue 0V/GND wiring ](https://user-images.githubusercontent.com/6767397/151781388-3685f609-194b-4560-9ce8-fe3fb7136718.png)

![My switch](https://user-images.githubusercontent.com/6767397/152822705-5c7b7282-3714-4ba6-bb03-98b0e2312197.png)

this is mine, on the 5V line. Als connected the ground from the 5v to the ground of the USB line




I use BKwriter (bk_writer1.60) to program the firmware, the programming / reading procedure is simple. We start the activity in the program, then turn off the WB2S from the power supply (and only from the power supply) with the button, and after a while we restore the power supply.
NOTE: instead of repowering the chip, you can try resetting it with CEN pin
The program should continue running as in the screenshot:
![screenshot of bk_writer1.60](https://user-images.githubusercontent.com/6767397/151782345-f0d45095-aec3-4ed3-a3fe-326d3a8bf034.png)


![Reading](https://user-images.githubusercontent.com/6767397/152823209-9d90332a-61fb-4b89-85fd-c1b8ec6a1575.png)
Even thought the red cross is there, the time is still counting and progress is going on


![Reading done](https://user-images.githubusercontent.com/6767397/152823521-007c6cb6-8a85-4953-9058-9fb7eaea4a4b.png)
Reading is a success, even with the FTDI dongle. So I continued and got it to erase the flash, but then it did not write.
It got in a boot loop. I thought this was done for, but then I read in the thread this is not the end.
Get another baudrate and keep on trying. So I did, unsuccesfull for the FTDI dongle, but as the ones used by the original author where also CH340's, I thought, lets give that a go: SUCCESS :)
![Uploaded](https://user-images.githubusercontent.com/6767397/152824009-1730fc45-4f28-4fd4-ad5d-e48f29ab78b3.png)



And a wifi access point appears in my phone. Connection is done, got an IP, trying to access 192.168.4.1 but it just keeps loading, doesn't show anything. Well... not with Firefox on Android. **Chromium did just fine**!



So I set the wifi of my internet modem, that didn't work :( Some setting in the modem likely, had that before on other IOT devices.
So, I've set an hotspot on my phone, with the same credentials, and being closer, the WB2S connected to my phone. Set the credentials for my 'IOT' router: success!


Get it configured: didn't find any instructions but worked it out by trial and error:

Interpretet acronyms as
- rel : relay on device
- btn : button on device
- LED : Status LED on device
- *_n : inverted state of mentioned device
- Open text field: NO CLUE WHAT THIS IS FOR, it looses its settings anyway

I've set all fields in the config page to Rel, saved, and a lot happenned, the relay switched a number of times quicly, and the homepage only showed 1 toggle status and: triggered correctly! 
Next, going back to the config pahe, only a few fields were still set. Thats nice, reduced testing: unset one by one and test the /index?tgl=0 page to get the right one.
Then, set a few to LED, test the same toggle page to see whether the onboard led represent the right state.
Next set the btn to a few unset fields and see whether the relay triggers by pushing the button.

Mine looks like this: (Filled the open text fields for the bennifit of this screenshot
![config](https://user-images.githubusercontent.com/6767397/152880233-4e8b4164-c048-4335-b40b-7c7372b496b6.png)



All working fine with the current versions of the app, building locally is quick enough.
https://github.com/ExploWare/OpenBK7231T_App
Now able to do OTA updates, so once the serial wiring is used and the OpenBK7231 firmware is uploaded, any future builds can be done over the wifi  connection, and therefor the solderings can be undone directly.
