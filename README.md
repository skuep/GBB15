# GBB15
A gantry-mounted CAN stepper driver board for 15mm aluminum beams (mostly for PrintersForAnts printers). This is heavily work in progress.


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
| 1.5mm M.2 Thermal pad    | 1   | Cut to size 67x15mm | -- | https://www.amazon.de/-/en/Assorted-Thickness-Conductive-Silicone-Conductivity/dp/B07X38254H/ref=sr_1_11 |
