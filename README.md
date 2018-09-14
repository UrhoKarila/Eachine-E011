# E011 Alternate Firmware

## Included hardware

Included in your kits should be a variety of hardware. They are they following:

### E011 Quadcopter  
The main piece of equipment required for this operation. These are the E011 quads - the ones for the workshop were obtained [here](https://www.banggood.com/Eachine-E011-Mini-2_4G-Headless-Mode-With-60000RPM-716-Coreless-Motor-Toy-Brick-RC-Quadcopter-RTF-p-1135724.html?rmmds=search).  
They are also available on amazon, or from other sellers.

![Quadcopter](/images/quadcopter.jpg)

If you've obtained the quadcopter through the workshop, it should have been modified with a 4-pin header across the UART programming pins. 
The header is polarized, so take careful note of its orientation on the board. If yours is the opposite direction of what is shown here, you'll want to reverse the order of Jumper Wire connections to the ST-Link.
That header should look like this once installed: 

![Image of the header](/images/header.jpg)

### Transmitter
A small control transmitter included with the quadcopter. Replacing this usually means buying a new quadcopter -- transmitters are not sold seperately.  
If you have a radio with an XJT module bay, this can be replaced with multiprotocol modules that are compatible with the Bayang protocol.

![Transmitter image](/images/transmitter.jpg)


### Batteries
The stock battery for the E011 is a single cell 260mAh Lithium Polymer battery. If you've lost yours, they are available in small quantites from a variety of sellers. `260mah lipo` is a decent start for googling.

![Battery Image](/images/battery.jpg)

### Battery charger
Keep this around. It can be used to recharge the LiPo battery for the drone.
Replacement chargers should be available with most replacement batteries should you lose it.

![Charger image](/images/charger.jpg)

### ST-Link
The ST-Link v2 is used to write data from the computer to the quadcopter. These are widely available and can be found, for example, [here](https://www.ebay.com/sch/i.html?_from=R40&_trksid=p2380057.m570.l1313.TR12.TRC2.A0.H0.XST-Link.TRS0&_nkw=ST-Link&_sacat=0)

![Picture of STLinkv2](/images/ST-Link-v2-programmer-for-stm8-stm32.jpg)

As part of the utility of the ST-Link, there are two sets of wires to keep track of.

#### Jumper wires
4 female to female jumper wires that come with the ST-Link. Your colors may vary.

![Picture of female jumper wires](/images/jumper-wires.jpg)

#### Header jumper

Also in your kit is a jumper wire. This is harder to replace, so take care not to lose it.  
One end of the jumper is terminated with a fairly no-name 1.25mm pitch 4 pin polarized connector, similar to [this one](https://smile.amazon.com/1-25mm-Polarized-Connector-5-9inch-Skywalking/dp/B00HTIV6G8).  
The other is crimped into a 4-posiiton male connector compatible with the Jumper Wires included above.

![Header Jumper image](/images/header-jumper.jpg)

### Voltmeter

A small PCB with a 3-position 7-segment display. There should be a plug compatible with the battery -- this is a useful tool for determining battery voltage

![voltmeter image](/images/voltmeter.jpg)

### Lipo Safety bag

A small fire-resistant bag. Keep spare batteries in here when not in use to reduce the risk of battery fires.  
The one included in your kit is availble for purchase [here](https://www.banggood.com/Realacc-New-Model-Lipo-Battery-Explosion-Proof-Bag-10x12cm-for-RC-Quadcopter-Battery-p-1054590.html?rmmds=myorder&cur_warehouse=CN) but there are a wide variety of acceptable alternatives.

![Picture of a lipo battery bag](/images/lipo-bag.jpg)

### Spare properllers

Useful if you fly the drone enough to warrant replacing the motors. There are 2 different chiralities on these - bear that in mind when replacing them.

![image of propellers](/images/propellers.jpg)

### Screwdriver

Small philips-head screwdriver. Can be used for removing the screw that holds the battery bay door closed on your transmitter, or removing the screws that hold the flight controller to the frame.

![image of a screwdriver](/images/screwdriver.jpg)

### Off-brand LEGO-style knight

![Probably the most valuable piece of this kit. Take care not to lose him. What would you do without his slightly walleyed smirk?](/images/lego-knight.jpg)


## Compiling firmware

Much of this section is taken from the [source thread on RC groups](https://www.rcgroups.com/forums/showthread.php?2877480-Compilation-instructions-for-silverware#post37391059) and mirrored here for readibility.

## Software installation:

### Install Keil MDK from http://www2.keil.com/mdk5  

There ought to be a button labelled `Download MDK v*`. At the time of writing, it looks like this:  

![An image of the download button](/images/download.png)

Note: right click the downloaded file and select "run as administrator", or there may not be enough folder access rights.

### Download and install the "ST-Link Utility" from https://www.st.com/en/development-tools/stsw-link004.html
This will also install drivers for the ST-Link, and is used to upload the precompiled firmware to the board.

The download link is near the bottom of the page, and looks like this: 

![An image of the STLink download](/images/st-download.PNG)


## Firmware Compilation

Assuming the installation process was performed correctly, we will now use Keil to compile a "binary", file containing the firmware of the quadcopter. This step will transform the C code that we've written into a binary format grokkable by the quadcopter's cpu.

1. Download or clone the project files from this repository, if you haven't already.  
Unzip to a folder of your choice.

2. Open project file `/Silverware/silverware.uvprojx` in the Keil uVision program. This can be done by double-clicking on the file. 

    * If you'd prefer, there's also `/Silverware/acro_only.uvprojx` already included for those who wish. If you don't know what "Acro Only" means offhand, I'd recommend using `silverware.uvprojx` to begin with.  
	
	* If this is your first time opening the project, Keil uVision may ask to install device support if not present. If so, click "install", and wait for completion, which may take some time as it is downloaded.  
	  This should install the `STM32F030` CPU support pack. There's a package installer tool in the IDE that can be tried as well, should the autodownload fail.


3. Change the settings to your preference, generally using `config.h`.  
    For workshop-defaults, you'll want to enable `#USE_STOCK_TX` on line 120.  
    Set the protocol to one of the Bayang ones. It should be good as-is.

4. Compile the project and make sure there are zero errors reported in the lower part of the window. Use either the "build" toolbar icon, menu "project/build target" or "F7" hotkey. Keil will also save changes to files at his point.

5. Open the folder "/Silverware/Objects" ( name may change slightly) and find file "bwhoop.hex". This is the firmware file, to be flashed to the quadcopter in the next step.

## Flashing firmware

Much of this section is taken from the [source thread on RC groups](https://www.rcgroups.com/forums/showthread.php?2876797-Boldclash-bwhoop-B-03-opensource-firmware) and mirrored here for readibility.

Flashing is the process of saving the opensource firmware to the board, so that it can be used. It's not very hard, and the board usually does not break unless power is connected incorrectly or the step in bold is performed differently.

### Preparing the hardware:

The quad is flashed using a ST-Link v2, included with your kit. The quad's flight controller is fitted with a socket:

![Picture of socket](/images/header-jumper-connections.jpg)

The Header Jumper and Jumper Wires should be connected to the ST-Link. The Header Jumper wires should (through the Jumper Wires) be connected as such:  

| Color   | Pin                 |  
| ------- | ------------------: |  
| Red     | GND                 |  
| Black   | 5V or disconnected  |  
| Yellow  | SWDIO               |  
| White   |	SWCLK               |  

Note: the black and red wires aren't attached to their conventional connections.

If you're using the included Header Jumper, it should plug neatly into the Jumper Wires. When attached, it should look like this:

![Quadcopter attached to the Header Jumper, Jumper Wires, and ST-Link](/images/quadcopter-stlink-connection.jpg)

*Do Not* connect both the +5V pin if the battery is also connected. This *will* damage the board

If connector is not available, wires can be soldered to the board instead. Note that the CLK and DAT labels on the Flight Controller are reversed.  
3 wires are needed, Ground ( GND) , CLK ( SWCLK on stlink) and DAT( SWDIO on stlink). Connect the pads / plug to the equivalent place on the ST-Link: GnD <-> Gnd , DAT <-> SWCLK and CLK <-> SWDIO.

At this stage, we'll assume you have a compiled firmware you wish to flash. This may be from the Firmware Compilation step above, or it may be a precompiled firmware. Either way, make sure you have something to upload after erasing the factory firmware.

### Erasing the factory firmware:

If this is the first time you're flashing new firmware, the factory firmware has to be erased. This step only needs to be performed once, it does not need to be done every time the board is flashed.

The pristine factory firmware cannot be restored after this step. However, there is a factory-like firmware included in the `/bin/` folder of this repository.

1. Connect the board to the ST-Link. Do not connect the 5V pin from the ST-Link - we'll be getting power from a battery for this step.
    You need to complete the next step within a few seconds because current firmware repurposes the programming port to control a camera. If you can't connect, cycle power and try again.

2. Using the St-utility, connect to the board. You should get a message saying the board is protected ( "Cannot read memory").

3. In the Target menu, select "Option bytes".

    1. Select "Level 0" ( Should have been Level 1 originally )  
    Do not select an incorrect Level and apply it as it will kill the board.

	2. **Double check you have "level 0" selected**

	3. Click Apply ( this will erase the factory firmware - click cancel instead to abort)

4. At this the factory firmware should erased. Click disconnect, and remove power from the board. You can now proceed to the next part.

### Flashing a new firmware:

1. Connect the ST-Link to the board, and connect power. This may use either the 5V pin or the battery, **but not both**.

2. Open the ST-Link Utility program , and press the "connect" button. The program should show connection progress in the bottom part.

3. In the target menu, select "Program and verify" or "program". A popup will ask for the file to program. Select the firmware downloaded or compiled `.hex` firmware file here. After the file selection, click start, and wait a few seconds for the process to complete.

The new firmware is flashed at this point, and the drone is ready to be used.


### Gestures
The firmware uses "gestures" to activate certain features, currently accelerometer calibration, acro / level mode switch and pid gestures ( field tuning )

Gestures reference:
http://sirdomsen.diskstation.me/dokuwiki/doku.php?id=gestures

### Wiki
http://sirdomsen.diskstation.me/dokuwiki/doku.php?id=start


### 02.07.17
* initial code
