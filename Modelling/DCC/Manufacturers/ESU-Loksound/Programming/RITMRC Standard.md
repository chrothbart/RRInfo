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
| 3 | Coupler* | Sound |
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
| 22 | Front Dim/Ditchlights\*\* | ProtoThrottle |
| 23 | Front Bright/Ditchlights\*\* | ProtoThrottle |
| 24 | Reverser Center | ProtoThrottle | Yes |
| 25 | Random 6 (Shutters) | Random |
| 26 | Rear Dim/Ditchlights\*\* | ProtoThrottle |
| 27 | Rear Bright/Ditchlights\*\* | ProtoThrottle |
| 28 | Not Currently Used |
| 29 | Not Currently Used |
| 30 | Coast | Motor Control | | For dynamic sound control |
| 31 | Notch 8 | Motor Control | | For dynamic sound control |

\* Custom coupler logic should be used, information coming soon

\*\* See ProtoThrottle notes on condensed lighting control

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

Section Coming Soon

## Release Notes

* 2026-05-04
  * Initial document creation
