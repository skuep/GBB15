# GBB15
A gantry-mounted CAN stepper driver board for 15mm aluminum beams (mostly for PrintersForAnts printers). 

This board can be used e.g. for the Micron to have A/B motor controllers directly at the gantry. This reduces the number of wires required to be routed to the gantry.

Additionally, this board can be used for Voron-0 or Tridents as a USB to CAN converter (using the Klipper CAN bridge option) with **included** A/B motor controllers.

![GBB15 Photo](images/GBB15.jpg?raw=true "GBB15 Photo")

![PCB front side](images/PCB-front.png?raw=true "PCB front side")
![PCB back side](images/PCB-back.png?raw=true "PCB back side")
![Micron Gantry](images/Micron-Gantry.png?raw=true "Micron Gantry")

# JLCPCB Specifications
0. Upload ``GERBER-GBB15.zip``
1. Select Qty
2. PCB Thickness 1.6mm
3. LeadFree HASL
4. Enable PCB Assembly
5. Choose **Economic**
6. Select Assembly Side **Bottom Side**
7. Tooling holes: **Added by Customer**
8. Press **Confirm**
9. Upload ``BOM-GBB15.csv``and ``CPL-GBB15.csv``
10. Check that all parts are in stock and click **NEXT**
11. Look for odd placement in the preview, select **NEXT**

# Additionally needed LCSC parts
You can order these on LCSC

| Part                     | Quantity | Notes  | LCSC Part Number | Link  | 
| ------------------------ | :-: | ----------- | ----------- |----------- |
| 100u/35V/105°C capacitor | 2   |             | C3035369    | https://www.lcsc.com/product-detail/Aluminum-Electrolytic-Capacitors-Leaded_Rubycon-35YXJ100MFFCT1-6-3X11_C3035369.html |
| MicroFit 3.0 connector   | 2   | Tin or Gold | C277721 or C563821 | https://www.lcsc.com/product-detail/Wire-To-Board-Wire-To-Wire-Connector_MOLEX-430450412_C277721.html or https://www.lcsc.com/product-detail/Pre-ordered-Connectors_MOLEX-430450413_C563821.html |
| JST PH 2.0mm 2-Pin conn. | 4   | Endstop/Thermistors | C131337 | https://www.lcsc.com/product-detail/Wire-To-Board-Wire-To-Wire-Connector_JST-Sales-America-B2B-PH-K-S-LF-SN_C131337.html |
| JST XH 2.5mm 4-Pin conn. | 2   | Steppers    | C144395 | https://www.lcsc.com/product-detail/Wire-To-Board-Wire-To-Wire-Connector_JST-Sales-America-B4B-XH-A-LF-SN_C144395.html |
| Vertical THT USB-C conn. | 1   |             | C3151747 | https://www.lcsc.com/product-detail/USB-Connectors_SHOU-HAN-TYPE-C-16PLC-H10-0_C3151747.html |
| CAN termination resistor | 1   | Optional!   | C17909 | https://www.lcsc.com/product-detail/Chip-Resistor-Surface-Mount_UNI-ROYAL-Uniroyal-Elec-1206W4F1200T5E_C17909.html |
| Tactile Switches         | 2   | Boot/Reset  | C255802 | https://www.lcsc.com/product-detail/Tactile-Switches_HYP-Hongyuan-Precision-1TS002E-2500-2501-CT_C255802.html |
| Heat Sink                | 2   | Standard TMC2209 heatsink | C5250902 or C5250879 | https://www.lcsc.com/product-detail/Heat-sink-heatsink_wenhaoyongshun-D11-01-02_C5250902.html or https://www.lcsc.com/product-detail/Heat-sink-heatsink_wenhaoyongshun-F12-04-05_C5250879.html |
| 1.5mm M.2 Thermal pad    | 1   | Cut to size 67x15mm | -- | https://www.amazon.de/-/en/Assorted-Thickness-Conductive-Silicone-Conductivity/dp/B07X38254H (DE) or https://www.amazon.com/ADWITS-12-Pack-Conductive-Silicone-Conductivity/dp/B07X81PRK7 (US) |

# Quick Guide
1. Solder top components (order: 120R resistor (if required), switches, 4x JST PH 2.0mm, 2x JST XH2.5mm, 2x MicroFit 3.0 2x2, USB connector, Capacitors & attach heatsinks)
1. Attach 1.5mm heat gap filler pad to bottom, cut to size if required using a sharp knife
1. Poke screw holes into the gap filler pads
1. Use M3x8mm screws to attach to bottom of gantry frame (see above)

Note that thermal connection to the 15mm beam is required! Otherwise the Stepper motor controllers might overheat.

# Firmware
1.  Plug in USB cable and connect 24V power, the green LED on the bottom side (Rev. B and up) must light up.
    Hold BOOT pushbutton, while tapping the RESET pushbutton and release all buttons. This is the sequence to set the GBB15 into DFU mode.

    **IMPORTANT**: STMicro decided on the entire STM32G0 series to set the ``nBOOT_SEL`` option bit and thus disable the ``BOOT0`` pin by default.
    This means you would have only one shot at uploading a bootloader, while the flash is empty. If you choose to re-program the bootloader (e.g. other CAN speed), you cannot get into the DFU mode anymore.
    For enabling the "legacy" ``BOOT0`` pin functionality, you have two options.
    1. You can use STMicro's STM32CubeProgrammer software to disable the ``nBOOT_SEL`` option bit by following the screenshot [here](doc/BOOT0-Via-STM32CubeProgrammer.png).
    1. Or if you prefer ``dfu-util``, which is included (or easily installed) in major linux distros (e.g. Raspberry Pi OS), you can run the ``enable_boot.hex`` firmware from [here](https://github.com/olikraus/stm32g031/tree/main/enable_boot0). 
       It will clear the ``nBOOT_SEL`` option bit from within the firmware, if it is not yet cleared.
       Use the following commands to upload the file.
       ```` 
           objcopy --input-target=ihex --output-target=binary enable_boot0.hex enable_boot0.bin # Convert hex to binary
           sudo dfu-util -a 0 -d 0483:df11 --dfuse-address 0x08000000:leave -D enable_boot0.bin
       ````
       After waiting for a second, the ``BOOT0`` pin is now enabled again and you can continue normally. 

       **NOTE:** Before continuing, remember to set the GBB15 into DFU mode again using the BOOT/RESET pushbuttons (see above).

1.  Build CanBoot according the instructions on the [CanBoot](https://github.com/Arksine/katapult) github page. 
    See [this](doc/GBB15-CanBoot.png) image for CanBoot menuconfig settings to use.

1.  Build the Klipper firmware according to the instructions on the [Klipper](https://www.klipper3d.org/Installation.html#building-and-flashing-the-micro-controller) github page.
    See [this](doc/GBB15-klipper.png) image for Klipper menuconfig settings to use.

1.  Upload CanBoot to the GBB15 using either STM32CubeProgrammer (if you have it installed) or using ``dfu-util`` with these commands.
    ````
        sudo dfu-util -a 0 -d 0483:df11 --dfuse-address 0x08000000:leave -D out/canboot.bin
    ````
    The device is now booted into CanBoot mode and can be accessed using the CanBoot tools.

1.  Upload the Klipper firmware to the GBB15 using either CanBoot's ``flashtool.py`` as described [here](https://github.com/Arksine/katapult#uploading-klipper) or Klipper's ``make flash`` (uses ``dfu-util``).
    1.  Using ``flashtool.py`` via CAN. Run these commands from the ``CanBoot/scripts`` directory
        ````
            python3 flashtool.py -i can0 -q # Check the UUID of the GBB15 board
            python3 flashtool.py -i can0 -f ~/klipper/out/klipper.bin -u <uuid> # Insert UUID here
        ````
    1.  Using ``make flash`` via Klipper Makefile. Run this command from the ``klipper`` directory.

        **NOTE:** For this, you need to set the GBB15 into DFU mode again using the BOOT/RESET pushbuttons (see above).
        ````
            make flash FLASH_DEVICE=0483:df11
        ````

# Pinout
This image shows the pinout of the GBB15 PCB. The CAN connector layout is compatible to the BigTreeTech EBB36 board (among others).
![GBB15 Pinout](doc/GBB15-Pinout.png?raw=true "GBB15 Pinout")

# Klipper configuration
````
[mcu gantry_mcu]
canbus_interface: can0
canbus_uuid: f7b6d9f26b4a

...

[temperature_sensor chamber]
sensor_type: Generic 3950
sensor_pin: gantry_mcu:PA1 # SMD thermistor on GBB15 PCB
#sensor_pin: gantry_mcu:PA3 # Alternatively, this is STOP1 connector if it is not used otherwise
#sensor_pin: gantry_mcu:PA2 # Alternatively, this is STOP2 connector if it is not used otherwise (for Rev. B and up) 
pullup_resistor: 4700

[temperature_sensor Motor_A]
sensor_type: Generic 3950
sensor_pin: gantry_mcu:PB11 # TH_A on GBB15 PCB
pullup_resistor: 4700

[temperature_sensor Motor_B]
sensor_type: Generic 3950
sensor_pin: gantry_mcu:PB12 # TH_B on GBB15 PCB
pullup_resistor: 4700

[temperature_sensor gantry_temp]
sensor_type: temperature_mcu
sensor_mcu: gantry_mcu
min_temp: 0
max_temp: 100

...

[stepper_y]
step_pin: gantry_mcu:PB2
dir_pin: gantry_mcu:PB1
enable_pin: !gantry_mcu:PB13
endstop_pin: gantry_mcu:PA3 # This is the STOP1 connector on GBB15 PCB
#endstop_pin: gantry_mcu:PA2 # Alternatively, this is STOP2 connector (for Rev. B and up, PC13 on Rev. A)
...

[tmc2209 stepper_y]
uart_pin: gantry_mcu:PA4
sense_resistor: 0.110
diag_pin: gantry_mcu:PB0
...

[stepper_x]
step_pin: gantry_mcu:PB7
dir_pin: gantry_mcu:PB8
enable_pin: !gantry_mcu:PB9
endstop_pin: gantry_mcu:PA2 # This is STOP2 connector (for Rev. B and up, PC13 on Rev. A)
#endstop_pin: gantry_mcu:PA3 # Alternatively, this is the STOP1 connector on GBB15 PCB
...

[tmc2209 stepper_x]
uart_pin: gantry_mcu:PB3
sense_resistor: 0.110
diag_pin: PB5
...

````

"Chamber" thermistor in this configuration is a SMD thermistor on the PCB. It reads probably 15-20°C above the actual chamber temperature, when the stepper motors are enabled and running. 
- To get better chamber temp readings, you can use TH_A or TH_B to connect a remote thermistor (in case you don't need those for monitoring the motor temperatures). 
- Alternatively you can (mis-)use the STOP1 (All revisions) and STOP2 (Rev. B and up) pins for analog readings. Easiest is a through-hole 100K B3950 NTC crimped into a JST PH2.0 and connected to one of the mentioned header pins.
