# Seven Segment Display Reloaded

Uses the overlay feature to create a configurable seven segment display.
Optimized for maximum configurability and use with seven segment clocks by parallyze (https://www.instructables.com/member/parallyze/instructables/)
Very loosely based on the existing usermod "seven segment display".


## Installation

Add the compile-time option `-D USERMOD_SSDR` to your `platformio.ini` (or `platformio_override.ini`) or use `#define USERMOD_SSDR` in `my_config.h`.

For the auto brightness option, the usermod SN_Photoresistor has to be installed as well. See SN_Photoresistor/readme.md for instructions.

## Settings
All settings can be controlled via the usermod settings page.
Part of the settings can be controlled through MQTT with a raw payload or through a json request to /json/state.

### enabled
Enables/disables this usermod

### inverted
Enables the inverted mode in which the background should be enabled and the digits should be black (LEDs off)

### Colon-blinking
Enables the blinking colon(s) if they are defined

### enable-auto-brightness
Enables the auto brightness feature. Can be used only when the usermod SN_Photoresistor is installed.

### auto-brightness-min / auto-brightness-max
The lux value calculated from usermod SN_Photoresistor will be mapped to the values defined here.
The mapping, 0 - 1000 lux, will be mapped to auto-brightness-min and auto-brightness-max

WLED current protection will override the calculated value if it is too high.

### Display-Mask
Defines the type of the time/date display. 
For example "H:m" (default)
- H - 00-23 hours
- h - 01-12 hours
- k - 01-24 hours
- m - 00-59 minutes
- s - 00-59 seconds
- d - 01-31 day of month
- M - 01-12 month
- y - 21 last two positions of year
- Y - 2021 year
- : for a colon

### Remove preceding zero of hours
Remove o not remove preceding zero of hour digits

### LED-Numbers
- LED-Numbers-Hours
- LED-Numbers-Minutes
- LED-Numbers-Seconds
- LED-Numbers-Colons
- LED-Numbers-Day
- LED-Numbers-Month
- LED-Numbers-Year

See following example for usage.


## Example

Example of an LED definition:
```
  <  A  >
/\       /\
F        B
\/       \/
  <  G  >
/\       /\
E        C
\/       \/
  <  D  >
```

LEDs or Range of LEDs are separated by a comma ","

Segments are separated by a semicolon ";" and are read as A;B;C;D;E;F;G

Digits are separated by colon ":" -> A;B;C;D;E;F;G:A;B;C;D;E;F;G

Ranges are defined as lower to higher (lower first)

For example, a clock definition for the following clock (https://www.instructables.com/Lazy-7-Quick-Build-Edition/) is

- hour "59,46;47-48;50-51;52-53;54-55;57-58;49,56:0,13;1-2;4-5;6-7;8-9;11-12;3,10"

- minute "37-38;39-40;42-43;44,31;32-33;35-36;34,41:21-22;23-24;26-27;28,15;16-17;19-20;18,25"

or

- hour "6,7;8,9;11,12;13,0;1,2;4,5;3,10:52,53;54,55;57,58;59,46;47,48;50,51;49,56"

- minute "15,28;16,17;19,20;21,22;23,24;26,27;18,25:31,44;32,33;35,36;37,38;39,40;42,43;34,41"

depending on the orientation.

# Example details:
hour "59,46;47-48;50-51;52-53;54-55;57-58;49,56:0,13;1-2;4-5;6-7;8-9;11-12;3,10"

there are two digits separated by ":"

- 59,46;47-48;50-51;52-53;54-55;57-58;49,56
- 0,13;1-2;4-5;6-7;8-9;11-12;3,10

In the first digit, 
the **segment A** consists of the LEDs number **59 and 46**., **segment B** consists of the LEDs number **47, 48** and so on

The second digit starts again with **segment A** and LEDs **0 and 13**, **segment B** consists of the LEDs number **1 and 2** and so on

### first digit of the hour
- Segment A: 59, 46
- Segment B: 47, 48
- Segment C: 50, 51
- Segment D: 52, 53
- Segment E: 54, 55
- Segment F: 57, 58
- Segment G: 49, 56

### second digit of the hour

- Segment A: 0, 13
- Segment B: 1, 2
- Segment C: 4, 5
- Segment D: 6, 7
- Segment E: 8, 9
- Segment F: 11, 12
- Segment G: 3, 10

# Led mapping
Use [LED mapping](https://kno.wled.ge/advanced/mapping/) to more easily adjust segments and create the correct direction of effects. You can use different ledmapX.json files for different effect direction. For some effects it's better to have the LEDs in one segment in sequence.

For example, a files ledmapX.json for the clock [Lazy 7 Quick Build Edition](https://www.instructables.com/Lazy-7-Quick-Build-Edition/) at depending on the orientation is

ledmap1.json
```
{"map":[
52,53,6,7,
51,50,54,55,5,4,8,9,
49,56,3,10,
48,47,57,58,2,1,11,12,
46,59,0,13,
45,30,29,14,
44,31,28,15,
43,42,32,33,27,26,16,17,
41,34,25,18,
40,39,35,36,24,23,19,20,
38,37,22,21
]}
```

ledmap2.json
```
{"map":[
51,50,48,47,43,42,40,39,
52,53,49,56,46,59,44,31,41,34,38,37,
54,55,57,58,45,30,32,33,35,36,
5,4,2,1,29,14,27,26,24,23,
6,7,3,10,0,13,28,15,25,18,22,21,
8,9,11,12,16,17,19,20
]}
```