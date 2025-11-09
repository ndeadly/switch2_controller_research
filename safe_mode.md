# Safe Mode

Switch 2 controllers have a hidden "Safe Mode" that can be entered by pressing a special key combination. This mode exposes a single vendor-specific interface over USB with an input and ouput endpoint for exchanging serial data.

To enter safe mode, press and hold the 3-button key combination for the controller and then release the buttons one at a time in the order written (see table below). If done correctly, the 1st and 4th player LEDs will light up and the device will enumerate over USB as `Nintendo Safe Mode Device` with its own product ID. Safe mode can be exited by pressing the `SYNC` button again.

| Controller              | Safe Mode Product ID | Key Combo           |
| ---                     | ---                  | ---                 |
| JoyCon 2 (R)            | 0x2070               | `ZR`+`PLUS`+`SYNC`  |
| JoyCon 2 (L)            | 0x2071               | `ZL`+`MINUS`+`SYNC` |
| Pro Controller 2        | 0x2072               | `ZR`+`PLUS`+`SYNC`  |
| Gamecube NSO Controller | 0x2074               | `Z`+`START`+`SYNC`  |

## Safe Mode Commands

The safe mode controller accepts serial data over the output endpoint of the exposed vendor interface. Sending >1 byte may be interpreted as multiple commands and result in several responses on the input endpoint. A response of `15` seems to indicate an invalid command.

Below is a table of seemingly valid commands observed when fuzzing the output endpoint.

| Command ID | Usage          | Example Command | Example Reponse | Comment                                                            |
| ---        | ---            | ---             | ---             | ---                                                                |
| 0x01       | Unknown        | `01`            | `0D 01`         | Possibly safe mode firmware version?                               |
| 0x02       | Unknown        | `02`            | `01`            |                                                                    |
| 0x03       | Unknown        | `03`            | `06`            |                                                                    |
| 0x04       | Unknown        | `04`            | `06`            |                                                                    |
| 0x05       | Get Product ID | `05`            | `69 20`         | Gets the original (non safe-mode) product ID of the device         |
| 0xAA       | Unknown        | `AA`            |                 | Reading input times out indicating additional data may be expected |
| 0xAB       | Unknown        | `AB`            |                 | Reading input times out indicating additional data may be expected |
