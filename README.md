# marstek-venus-c-hardware-notes
Reverse engineering notes, hardware analysis, and extracted content from the Marstek Venus C plug-in home battery 

Opening the unit:
- Using a cutter knife, cut along the silicone seal between the acrylic front panel and the metal case. Insert the knife about 15mm and try not to scratch the backside of the acrylic plate as it is printed on the back side (if you want to keep it nice looking)
- The acrylic plate is fixed to the metal case with a generous amount of double-sided foam tape. To remove the plate, pry with you fingers between case and plate. After a while, the foam tape will come off or rip in halves. Takes time and some cursing.
- The metal case has a 'front door' held in place by 3 countersunk M4 screws, which may be partly covered by the foam tape
- Limited access is possible by removing the 4 hex screws of the connector panel and gently pulling it out

Misc findings:
- The whole unit has a decent build quality, the components are from ok brands, the electrolytic capacitors may be a little on the cheap side
- The PCBs are partly painted with a hard coating to prevent moisture damage. This makes probing signals on fine-pitch components difficult.

General topology:
- The on/off button on the connector panel is connected to the BMS and switches the battery voltage to the Converter PCB. This means that the Converter/Control PCBs are powered down and are safe to work on when the green light of the button is off.
- The BMS is connected to the Control PCB via CAN bus and RS485. This RS485 connection is the same as the external RS485. The function of the RS485 connection is somewhat unclear. The whole device is a Modbus slave, maybe the BMS answers on some adresses and the Control on others. As long as there is Modbus communication, the device does not shut down. Maybe the RS485 connetion is used as keep-awake function.
- The DC-link is the centerpiece of the system

Control PCB:
- main control MCU STM32G474VET6
- SWD pin header
- reset button
- GoldCap on MCU Vbatt pin. Likely to keep RAM / RTC contents
- separate EEPROM for configuration and maybe calibration
- auxiliary isolated flyback to convert battery voltage to DC-link voltage
- auxiliary isolated flyback to convert DC-link voltage to several 12V-ish voltages on the primary and secondary side
- several quad opamps used for analog signal conditioning of the many voltage and current measurements
- several HC08 AND-gates between MCU and the isolated gatedrivers on the converter PCB. Probably some kind of emergency-shutdown when the software runs amok.

LED PCB:
- the fuel gauge segments are made of 4 LEDs, the status segments are made of 2 LEDs
- driver transistor between main MCU GPIOs and LEDs
- each segment is connected to a GPIO
- small buck converter 12v to LED drive voltage

Converter PCB:
- bidirectional LLC-converter between battery and DC-link
- bidirectional IGBT Fullbridge between DC-link and AC in/output acting both as grid-tied and off-grid inverter as well as PFC in charging state
- the LLC transformer (and maybe the resonace indutance) and AC output filter inductor is off the PCB and potted near the external heatsink for cooling

Cell stack and BMS:
