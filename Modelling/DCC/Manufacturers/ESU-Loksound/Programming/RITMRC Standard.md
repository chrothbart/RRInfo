# RITMRC ESU-Loksound v5 Programming Standard
## Overview
Documented below is the current RITMRC standard for configuration of an ESU/Loksound v5 decoder, including the function mapping and relevant configuration steps.

Please note this standard is still undergoing testing. Please expect updates, and check the release notes for specific changes.

## Functions

Note: the below is an operators reference. See the configuration sections for the actual function mapping.

| # | Function | Type | Active in Consist | Notes |
| - | -------- | ---- | ----------------- | ----- |
| 0 | Headlight | Lighting | Yes |
| 1 | Bell | Sound |
| 2 | Horn | Sound |
| 3 | Coupler [^Coupler] | Sound |
| 4 | Brakes | Motor Control | Yes |
| 5 | Dynamic Brakes | Motor Control | Yes |
| 6 | Heavy Load | Motor Control | Yes |
| 7 | Dimmer | Lighting |
| 8 | Prime Mover | Motor Control | Yes |
| 9 | Ditch Lights | Lighting |
| 10 | Number Boards | Lighting |
| 11 | Marker Lights | Lighting |
| 12 | Switching Lighting | Lighting | Yes | Both headlights dim |
| 13 | Extra Lighting | Lighting |
| 14 | Extra Lighting | Lighting |
| 15 | Extra Lighting | Lighting |
| 16 | Extra Lighting | Lighting |
| 17 | Random 1 (Radiator) | Random |
| 18 | Random 2 (Sanders) | Random |
| 19 | Random 3 ( Short Air Let Off) | Random | Yes[^PTAIR] | ProtoThrottle air test |
| 20 | Random 4 (Compressor) | Random | Yes[^PTAIR] | ProtoThrottle air test |
| 21 | Random 5 (Air Dryer) | Random |
| 22 | Front Dim/Ditchlights [^PTLight] | ProtoThrottle |
| 23 | Front Bright/Ditchlights [^PTLight] | ProtoThrottle |
| 24 | Reverser Center | ProtoThrottle | Yes |
| 25 | Random 6 (Shutters) | Random |
| 26 | Rear Dim/Ditchlights [^PTLight] | ProtoThrottle |
| 27 | Rear Bright/Ditchlights [^PTLight] | ProtoThrottle |
| 28 | Not Currently Used |
| 29 | Not Currently Used |
| 30 | Coast | Motor Control | | For dynamic sound control |
| 31 | Notch 8 | Motor Control | | For dynamic sound control |

[^Coupler]: Custom coupler logic should be used, information coming soon

[^PTLight]: See ProtoThrottle notes on condensed lighting control

[^PTAIR]: Used by ProtoThrottle Air Test feature

## Configuration

Below are all the steps required to change a default LokSound file to match the RITMRC standard. Generally speaking, only the setting that need to be changed will be mentioned.

### Address

#### Active functions in consist mode

The following should be active:
* F4 Brake
* F5 Dynamic Brake
* F6 Heavy Load
* F8 Prime Mover
* F12 Switching Lighting
* F19 Short Air Let Off[^PTAIR]
* F20 Compressor[^PTAIR]
* F24 Reverser

### Analog Settings

#### Enable DC Analog Mode

Unless it is essential that the locomotive be usable on a DC layout, disable this. Leaving this enabled can cause runaway locomotives during certain layout issues.

`Enable DC analog mode: disabled`

### Brake Settings

#### Brake functions

Currently all brake functions are left at their default values:

| Brake | Reduction | Max Speed |
| ----- | --------- | --------- |
| Brake 1 | 128 / 50.2% | 126 |
| Brake 2 | 128 / 50.2% | 126 |
| Brake 3 | 240 / 94.12% | 126 |

Note: if Max Speed is set to 0, the locomotive will be held at stop until brake is released. By setting Max Speed to 126, the locomotive can be moved from a stop. If deceleration is set for coasting, requiring a brake to stop, then normal stopping behavior can be achieved by leaving Max Speed at 126 and running with the brake on.

### DCC Settings

#### RailCom settings

Historically this was set to disabled, but with the arrival of hardware that sanely supports RailCom, this is being left enabled.

`Leave all RailCom fields enabled`

#### Speed step mode

This should be configured correctly by default on v5 DCC decoders, but ensure the following:

```
* Deselect 'Detect speed step mode automatically'
* Select 'Use 28 or 128 speed steps'
```

14 speed step mode is a legacy mode, which should never be encountered. Disabling it removes the chance of unexpected behavior.

### Driving Characteristics

#### Acceleration and deceleration

RITMRC locomotives are configured to coast, requiring a brake to stop. This is needed for proper operation with a ProtoThrottle, and is preferred for more realistic operation with a regular throttle. To achieve this, configure the following:

`Time from maximum speed to stop: 255`

#### Load Adjustment

TBD: Changes may be needed here, further research is required.

#### Starting delay

Leaving this enabled will prevent your locomotive from begininning to move while startup sounds are playing. With the addition of the 'Persist function' setting, this is likely less necessary, but for now the standard is still to disable it.

`Delay starting if drive sound is enabled: disabled`

### Function Outputs

#### Configuring headlights for ProtoThrottle operation

```
Configure Front headlight dim:
 Output: Front light [2]
 Name:   Front Headlight (Dim)
 Output Mode: Dimmable headlight (fade in/out)
 Brightness: 5
 Special Functions: LED Mode

Configure Rear Headlight Dim:
 Output: Rear light [2]
 Name:   Rear Headlight (Dim)
 Output Mode: Dimmable headlight (fade in/out)
 Brightness: 5
 Special Functions: LED Mode
```

#### Configuring other lighting functions

See the following for instructions on configuring other lighting functions:

[Configuring Lighting for Loksound Decoder]

[Configuring Class Lights Using RGB LEDs]

### Function Settings

#### Random functions

Under the RITMRC standard, random function 1 is moved from F11 to F17. Ensure that this is changed both here, and under function mapping.

```
Random 1:
 Triggered function: F17
```

### Function Mapping

| Name              | F#  | D | M | Physical Outputs | Logical Functions | Sounds |
| ----------------- | --  | - | - | ---------------- | ----------------- | ------ |
| Front Headlight   | F0  | F |   | Front Light [1]  |
| Rear Headlight    | F0  | R |   | Rear Light [1]   |
| Bell              | F1  |   |   |                  |                   | [4] Bell |
| Horn              | F2  |   |   |                  | Grade Crossing    | [3] Horn |
| Coupler           | F3  |   |   |                  |                   | Coupler [^Coupler] |
| Brake             | F4  |   |   |                  | Brake 3           |          |
| Brake [1]         | not F4 | |  |                  | Disable Brake Sound  |
| Dynamic Brake     | F5  |   |   |                  | Shift 1 (Dynamics)   | [6] Dynamic Brakes |
| Heavy Load        | F6  |   |   |                  | Shift 6 (Heavy Load) |
| Dimmer            | F7  |   |   |                  | Dimmer            | [24] Short Air Let Off |
| Engine Sounds     | F8  |   |   |                  |                   | [1] Prime Mover Sound |
| Engine Sounds [2] | F8  |   | S |                  |                   | [29] Brake Automatic Set/Release |
| Engine Sounds [3] | F8, not F15 | | S |  Update This | Update This | [26] Starting Delay |
| Engine Sounds     | F8  |   | D |                  |                   | [25] Traction Motors |
| Engine Sounds [4] | not F8 | |  |                  |                   | [18] Air Dryers on Shutdown |
| Ditchlights*      | not F7, F9, not F12 | F | | Front Ditchlights |
| Ditchlights*      | not F7, F9, not F12 | R | | Rear Ditchlights |
| Number Boards*    | F10 |   |   | Number Boards    |
| Marker Lights*    | F11 |   |   | Marker Lights    |
| Switching Mode*   | F12 |   |   | Front & Rear Headlight | Dimmer |
| Extra Lighting*   | F13 |   |   | Extra Lighting   |
| Extra Lighting*   | F14 |   |   | Extra Lighting   |
| Extra Lighting*   | F15 |   |   | Extra Lighting   |
| Extra Lighting*   | F16 |   |   | Extra Lighting   |
| Random Sound 1    | F17 |   |   |                  |                   | [8] Radiator |
| Random Sound 2    | F18 |   |   |                  |                   | [13] Sanding Valve |
| Random Sound 3    | F19 |   |   |                  |                   | [24] Short Air Let Off |
| Random Sound 4    | F20 |   |   |                  |                   | [7] Air Compressor |
| Random Sound 5    | F21 |   |   |                  |                   | [17]  Air Dryer |
| Front Dim [^PTLight] | F22, not F23 | | | Front Light [2] (Dim) |
| Front Bright [^PTLight] | not F22, F23 | | | Front Light [1] |
| Front Bright/Ditch | F22, F23 | | | Front Headlight [1], Front Ditchlights |
| Reverser Center   | F24 |   |   |                  | Shift 5 (Reverser) | [20] Reverser Center |
| Random Sound 6    | F25 |   |   |                  |                   | [32] Shutters |
| Rear Dim [^PTLight] | F26, not F27 | | | Rear Light [2] (Dim) |
| Rear Bright [^PTLight] | not F26, F27 | | | Front Light [1] |
| Rear Bright/Ditch | F26, F27 | | | Rear Light [1], Rear Ditchlights |
| Coast             | F30 |   |   |                  | Shift 3 (Coast) |
| Run 8             | F31 |   |   |                  | Shift 2 (Run 8) |

### Identification

This is currently unused, but that should probably change.

### Compatibility

#### Broadway Limited Steam Engine Control

[Configure LokSound for BLI Steam]

### Motor Settings

[LokSound Speed Matching]

### Smoke Unit

Section Coming Soon

### Special Options

#### Memory Settings

`Persist functions: enabled`

### Sound Settings

#### Steam Chuffs

[Configure LokSound for BLI Steam]

#### Brake Sounds

Todo: test these settings

#### Dynamic Sound Control

Section Coming Soon

### Sound Slot Settings

Section Coming Soon

## Release Notes


* 2026-05-08
  * Completed outline
* 2026-05-04
  * Initial document creation
