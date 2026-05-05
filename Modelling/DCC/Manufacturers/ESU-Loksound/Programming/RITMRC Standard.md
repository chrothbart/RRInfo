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
| 19 | Random 3 ( Short Air Let Off) | Random | Yes | ProtoThrottle air test |
| 20 | Random 4 (Compressor) | Random | Yes | ProtoThrottle air test |
| 21 | Random 5 (Air Dryer) | Random |
| 22 | Front Dim/Ditchlights\*\* | ProtoThrottle |
| 23 | Front Bright/Ditchlights\*\* | ProtoThrottle |
| 24 | Reverser Center | ProtoThrottle |
| 25 | Random 6 (Shutters) | Random |
| 26 | Rear Dim/Ditchlights\*\* | ProtoThrottle |
| 27 | Rear Bright/Ditchlights\*\* | ProtoThrottle |
| 28 | Not Currently Used |
| 29 | Not Currently Used |
| 30 | Coast | Motor Control | | For dynamic sound control |
| 31 | Notch 8 | Motor Control | | For dynamic sound control |

\* Custom coupler logic should be used, information coming soon

\*\* See ProtoThrottle notes on condensed lighting control

## Configuration

Section Coming Soon

## Release Notes

* 2026-05-04
  * Initial document creation
