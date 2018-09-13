# E011 Alternate Firmware

##Included hardware

Included in your kits should be a variety of hardware. They are they following:

###E011 Quadcopter  
The main piece of equipment required for this operation. These are the E011 quads - the ones for the workshop were obtained [here](https://www.banggood.com/Eachine-E011-Mini-2_4G-Headless-Mode-With-60000RPM-716-Coreless-Motor-Toy-Brick-RC-Quadcopter-RTF-p-1135724.html?rmmds=search).  
They are also available on amazon, or from other sellers.

![Placeholder Image](/images/placeholder.jpg)

If you've obtained the quadcopter through the workshop, it should have been modified with a 4-pin header across the UART programming pins. 
The header is polarized, so take careful note of its orientation on the board. If yours is the opposite direction of what is shown here, you'll want to reverse the order of Jumper Wire connections to the ST-Link.
That header should look like this once installed: 

![Placeholder image](/images/placeholder.jpg)

###Transmitter
A small control transmitter included with the quadcopter. Replacing this usually means buying a new quadcopter -- transmitters are not sold seperately.  
If you have a radio with an XJT module bay, this can be replaced with multiprotocol modules that are compatible with the Bayang protocol.

![Transmitter placeholder image](/images/placeholder.jpg)


###Batteries
The stock battery for the E011 is a single cell 260mAh Lithium Polymer battery. If you've lost yours, they are available in small quantites from a variety of sellers. `260mah lipo` is a decent start for googling.

![Placeholder Image](/images/placeholder.jpg)

###Battery charger
Keep this around. It can be used to recharge the LiPo battery for the drone.
Replacement chargers should be available with most replacement batteries should you lose it.

![Charger placeholder](/images/placeholder.jpg)

###ST-Link
The ST-Link v2 is used to write data from the computer to the quadcopter. These are widely available and can be found, for example, [here](https://www.ebay.com/sch/i.html?_from=R40&_trksid=p2380057.m570.l1313.TR12.TRC2.A0.H0.Xst-link.TRS0&_nkw=st-link&_sacat=0)

![Picture of STLinkv2](/images/st-link-v2-programmer-for-stm8-stm32.jpg)

As part of the utility of the ST-Link, there are two sets of wires to keep track of.

####Jumper wires
4 female to female jumper wires that come with the ST-Link. Your colors may vary.

![Picture of female jumper wires](/images/jumper-wires.jpg)

####Header jumper

Also in your kit is a jumper wire. This is harder to replace, so take care not to lose it.  
One end of the jumper is terminated with a fairly no-name 1.25mm pitch 4 pin polarized connector, similar to [this one](https://smile.amazon.com/1-25mm-Polarized-Connector-5-9inch-Skywalking/dp/B00HTIV6G8).  
The other is crimped into a 4-posiiton male connector compatible with the Jumper Wires included above.

![Placeholder image](/images/placeholder.jpg)

###Voltmeter

A small PCB with a 3-position 7-segment display. There should be a plug compatible with the battery -- this is a useful tool for determining battery voltage

![Placeholder image](/images/placeholder.jpg)

###Lipo Safety bag

A small fire-resistant bag. Keep spare batteries in here when not in use to reduce the risk of battery fires.  
The one included in your kit is availble for purchase [here](https://www.banggood.com/Realacc-New-Model-Lipo-Battery-Explosion-Proof-Bag-10x12cm-for-RC-Quadcopter-Battery-p-1054590.html?rmmds=myorder&cur_warehouse=CN) but there are a wide variety of acceptable alternatives.

![Picture of a lipo battery bag](/images/lipo-bag.jpg)

###Spare properllers

Useful if you fly the drone enough to warrant replacing the motors. There are 2 different chiralities on these - bear that in mind when replacing them.
![Placeholder image of propellers](/images/placeholder.jpg)

###Screwdriver

Small philips-head screwdriver. Can be used for removing the screw that holds the battery bay door closed on your transmitter, or removing the screws that hold the flight controller to the frame.
![Placeholder image of a screwdriver](/images/placeholder.jpg)

###Off-brand LEGO-style knight

Probably the most valuable piece of this kit. Take care not to lose him. What would you do without his slightly walleyed smirk?
![Placeholder image of knight](/images/placeholder.jpg)


##Compiling firmware

Much of this section is taken from the [source thread on RC groups](https://www.rcgroups.com/forums/showthread.php?2877480-Compilation-instructions-for-silverware#post37391059) and mirrored here for readibility.

###Software installation:

## Install Keil MDK from http://www2.keil.com/mdk5  

There ought to be a button labelled `Download MDK v*`. At the time of writing, it looks like this:  
[!An image of the download button](/images/download.png)
Note: right click the downloaded file and select "run as administrator", or there may not be enough folder access rights.


Assuming the installation process was performed correctly, we will now use Keil to compile a "binary", file containing the firmware of the quadcopter. This step will transform the C code that we've written into a binary format grokkable by the quadcopter's cpu.

1: Download the project files from this repo, if you haven't already. I'm not going to link it, because you're already here.  
Unzip to a folder of your choice.

2: Open project file `/Silverware/silverware.uvprojx` in the Keil uVision program. This can be done by double-clicking on the file. 
    If you'd prefer, there's also `/Silverware/acro_only.uvprojx` already included for those who wish. If you don't know what "Acro Only" means offhand, I'd recommend using `silverware.uvprojx` to begin with.  
If this is your first time opening the project, Keil uVision may ask to install device support if not present. If so, click "install", and wait for completion, which may take some time as it is downloaded.  
This should install the `STM32F030` CPU support pack. There's a package installer tool in the IDE that can be tried as well, should the autodownload fail.


3: Change the settings to your preference, generally using `config.h`.  
    For workshop-defaults, you'll want to enable `#USE_STOCK_TX` on line 120, as well as set the protocol to one of the Bayang ones. It should be good as-is.

4: Compile the project and make sure there are zero errors reported in the lower part of the window. Use either the "build" toolbar icon, menu "project/build target" or "F7" hotkey. Keil will also save changes to files at his point.

5: Open the folder "/Silverware/Objects" ( name may change slightly) and find file "bwhoop.hex". This is the firmware file, to be flashed to the quadcopter using the "st-link utility" program.

##Flashing firmware



The board pads ( near the middle ) are *reverse* labeled. The 4 pin connector is *correctly* labeled.

Thanks to rcg user "NotfastEnough" for figuring out the hardware connctions

### Factory firmware
The factory firmware can be flashed back after using this, it's in the bin folder

### Gestures
The firmware uses "gestures" to activate certain features, currently accelerometer calibration, acro / level mode switch and pid gestures ( field tuning )

Gestures reference:
http://sirdomsen.diskstation.me/dokuwiki/doku.php?id=gestures

### Flashing the firmware
Flashing instructions ( uses St-link Utility program ):
https://www.rcgroups.com/forums/showthread.php?2876797-Boldclash-bwhoop-B-03-opensource-firmware

Compiling instructions ( uses Keil uVision IDE):
https://www.rcgroups.com/forums/showthread.php?2877480-Compilation-instructions-for-silverware#post37391059


### Wiki
http://sirdomsen.diskstation.me/dokuwiki/doku.php?id=start


### 02.07.17
* initial code
