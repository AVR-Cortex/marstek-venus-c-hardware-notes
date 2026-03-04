# marstek-venus-c-hardware-notes
Reverse engineering notes, hardware analysis, and extracted content from the Marstek Venus C plug-in home battery 

Opening the unit:
- Using a cutter knife, cut along the silicone seal between the acrylic front panel and the metal case. Insert thr knife about 15mm and try not to scratch the backside of the acrylic plate as it is printed on the back side (if you want to keep it nice looking)
- The acrylic plate is fixed to the metal case with a generous amount of double-sided foam tape
- The metal case has a 'front door' held in place by 3 countersunk M4 screws, which may be partly covered by the foam tape

Misc findings:
- The whole unit has a decent build quality, the components are from ok brands, the electrolytic capacitors may be a little on the cheap side

General topology:
- The on/off button on the connector panel is connected to the BMS and switches the battery voltage to the Converter PCB. This means that the Converter/Control PCBs are powered down and are safe to work on when the green light of the button is off.

Control PCB:
- main control MCU STM32G474VET6
- SWD pin header
- reset button

Converter PCB:

Cell stack and BMS:
