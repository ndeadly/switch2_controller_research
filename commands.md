# Commands

### Command Structure

| Offset | Size | Value                     | Comment               |
| ---    | ---  | ---                       | ---                   |
| 0x0    | 0x8  | [Header](#command-header) | Command header        |
| 0x8    | 0-N  | Data                      | Optional command data |

### Command Header

| Offset | Size | Value           | Comment                                                           |
| ---    | ---  | ---             | ---                                                               |
| 0x0    | 0x1  | Command ID      | -                                                                 |
| 0x1    | 0x1  | Direction       | `0X91` = Host->Device (request), `0x01` = Device->Host (response) |
| 0x2    | 0x1  | Transport       | `0x00`=USB, `0x01`=Bluetooth                                      |
| 0x3    | 0x1  | Subcommand ID   | -                                                                 |
| 0x4    | 0x1  | Unknown         | -                                                                 |
| 0x5    | 0x1  | Data Length/ACK | Request: Length of data to follow, response: ACK                  |
| 0x6    | 0x2  | Reserved        | Always 0. Probably just padded out to 8 bytes                     |

### Command Types

| Command ID                              | Usage           |
| ---                                     | ---             |
| [0x01](#command-0x01---nfc)             | NFC             |
| [0x02](#command-0x02---flash-memory)    | Flash Memory    |
| [0x03](#command-0x03---initialisation)  | Initialisation  |
| [0x04](#command-0x04---unknown)         | Unknown         |
| [0x05](#command-0x05---unknown)         | Unknown         |
| [0x06](#command-0x06---unknown)         | Unknown         |
| [0x07](#command-0x07---unknown)         | Unknown         |
| [0x08](#command-0x08---unknown)         | Unknown         |
| [0x09](#command-0x09---player-leds)     | Player LEDs     |
| [0x0A](#command-0x0a---vibration)       | Vibration       |
| [0x0B](#command-0x0b---battery)         | Battery         |
| [0x0C](#command-0x0c---feature-select)  | Feature Select  |
| [0x0D](#command-0x0d---firmware-update) | Firmware Update |
| [0x0E](#command-0x0e---unknown)         | Unknown         |
| [0x0F](#command-0x0f---unknown)         | Unknown         |
| [0x10](#command-0x10---unknown)         | Unknown         |
| [0x11](#command-0x11---unknown)         | Unknown         |
| [0x12](#command-0x12---unknown)         | Unknown         |
| [0x13](#command-0x13---unknown)         | Unknown         |
| [0x14](#command-0x14---unknown)         | Unknown         |
| [0x15](#command-0x15---pairing)         | Pairing         |
| [0x16](#command-0x16---unknown)         | Unknown         |
| [0x17](#command-0x17---unknown)         | Unknown         |
| [0x18](#command-0x18---unknown)         | Unknown         |

## Command 0x01 - NFC

| Command | Subcommand | Usage | Example Request           | Example Response                        |
| ---     | ---        | ---   | ---                       | ---                                     |
| `0x01`  | `0x01`     |       |                           |                                         |
| `0x01`  | `0x02`     |       |                           |                                         |
| `0x01`  | `0x03`     |       |                           |                                         |
| `0x01`  | `0x04`     |       |                           |                                         |
| `0x01`  | `0x05`     |       |                           |                                         |
| `0x01`  | `0x06`     |       |                           |                                         |
| `0x01`  | `0x0C`     |       | `01 91 01 0c 00 00 00 00` | `01 01 01 0c 10 78 00 00` `61 12 50 0d` |

## Command 0x02 - Flash Memory

| Command | Subcommand | Usage        | Example Request                                                                                                                                                                                                                                     | Example Response                                                                                    |
| ---     | ---        | ---          | ---                                                                                                                                                                                                                                                 | ---                                                                                                 |
| `0x02`  | `0x01`     |              |                                                                                                                                                                                                                                                     |                                                                                                     |
| `0x02`  | `0x03`     |              |                                                                                                                                                                                                                                                     |                                                                                                     |
| `0x02`  | `0x04`     | Memory read  | `02 91 01 04 00 08 00 00` `10 7e 00 00 40 30 01 00`                                                                                                                                                                                                 | `02 01 01 04 10 78 00 00` `10 00 00 00 40 30 01 00 3b e0 d3 41 c6 60 6a bc 4d d7 a2 bb 71 1e dd 37` |
| `0x02`  | `0x05`     | Memory write | `02 91 01 05 00 48 00 00` `40 7e 00 00 00 c0 1f 00 b2 a1 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00` | `02 01 01 05 10 78 00 00` `00 00 00 00 00 c0 1f 00`                                                 |

## Command 0x03 - Initialisation

| Command | Subcommand | Usage               | Example Request                                                                               | Example Response                        |
| ---     | ---        | ---                 | ---                                                                                           | ---                                     |
| `0x03`  | `0x01`     |                     |                                                                                               |                                         |
| `0x03`  | `0x02`     |                     |                                                                                               |                                         |
| `0x03`  | `0x03`     |                     |                                                                                               |                                         |
| `0x03`  | `0x04`     |                     |                                                                                               |                                         |
| `0x03`  | `0x05`     |                     |                                                                                               |                                         |
| `0x03`  | `0x06`     |                     |                                                                                               |                                         |
| `0x03`  | `0x07`     | Store pairing info  | `03 91 01 07 00 16 00 00` `5e 11 85 eb f1 48 c1 27 80 67 1a fd 29 b8 00 e1 dd c5 19 b4 f0 54` | `03 01 01 07 10 78 00 00`               |
| `0x03`  | `0x08`     |                     |                                                                                               |                                         |
| `0x03`  | `0x09`     | Unknown             | `03 91 01 09 00 00 00 00`                                                                     | `03 01 01 09 10 78 00 00`               |
| `0x03`  | `0x0A`     | Select input report | `03 91 00 0a 00 04 00 00` `09 00 00 00`                                                       | `03 01 00 0a 00 f8 00 00`               |
| `0x03`  | `0x0C`     | Unknown             | `03 91 00 0c 00 04 00 00` `01 00 00 00`                                                       | `03 01 00 0c 00 f8 00 00`               |
| `0x03`  | `0x0D`     |                     | `03 91 00 0d 00 08 00 00` `01 00 31 7e c6 eb f1 48`                                           | `03 01 00 0d 00 f8 00 00` `01 00 00 00` |
| `0x03`  | `0x0F`     |                     |                                                                                               |                                         |

## Command 0x04 - Unknown


## Command 0x05 - Unknown

| Command | Subcommand | Usage   | Example Request | Example Response |
| ---     | ---        | ---     | ---             | ---              |
| `0x05`  | `0x01`     |         |                 |                  |

## Command 0x06 - Unknown

| Command | Subcommand | Usage   | Example Request                         | Example Response          |
| ---     | ---        | ---     | ---                                     | ---                       |
| `0x06`  | `0x01`     |         |                                         |                           |
| `0x06`  | `0x02`     |         |                                         |                           |
| `0x06`  | `0x03`     | Reboot? | `06 91 01 03 00 04 00 00` `01 00 00 00` | `06 01 01 03 10 78 00 00` |

## Command 0x07 - Unknown

| Command | Subcommand | Usage   | Example Request           | Example Response               |
| ---     | ---        | ---     | ---                       | ---                            |
| `0x07`  | `0x01`     | Unknown | `07 91 01 01 00 00 00 00` | `07 01 01 01 10 78 00 00` `00` |
| `0x07`  | `0x02`     |         |                           |                                |

## Command 0x08 - Unknown

## Command 0x09 - Player LEDs

| Command | Subcommand | Usage                  | Example Request                                     | Example Response          |
| ---     | ---        | ---                    | ---                                                 | ---                       |
| `0x09`  | `0x01`     |                        |                                                     |                           |
| `0x09`  | `0x02`     |                        |                                                     |                           |
| `0x09`  | `0x03`     |                        |                                                     |                           |
| `0x09`  | `0x04`     |                        |                                                     |                           |
| `0x09`  | `0x05`     |                        |                                                     |                           |
| `0x09`  | `0x06`     |                        |                                                     |                           |
| `0x09`  | `0x07`     | Set player LED pattern | `09 91 01 07 00 08 00 00` `01 00 00 00 00 00 00 00` | `09 01 01 07 10 78 00 00` |
| `0x09`  | `0x08`     |                        |                                                     |                           |

## Command 0x0A - Vibration

| Command | Subcommand | Usage                 | Example Request                                                                         | Example Response          |
| ---     | ---        | ---                   | ---                                                                                     | ---                       |
| `0x0A`  | `0x01`     |                       |                                                                                         |                           |
| `0x0A`  | `0x02`     | Play vibration sample | `0a 91 01 02 00 04 00 00` `03 00 00 00`                                                 | `0a 01 01 02 10 78 00 00` |
| `0x0A`  | `0x03`     |                       |                                                                                         |                           |
| `0x0A`  | `0x04`     |                       |                                                                                         |                           |
| `0x0A`  | `0x05`     |                       |                                                                                         |                           |
| `0x0A`  | `0x06`     |                       |                                                                                         |                           |
| `0x0A`  | `0x07`     |                       |                                                                                         |                           |
| `0x0A`  | `0x08`     | Send vibration data   | `0a 91 01 08 00 14 00 00` `01 ff ff ff ff ff ff ff ff 35 00 46 00 00 00 00 00 00 00 00` | `0a 01 01 08 10 78 00 00` |
| `0x0A`  | `0x09`     |                       |                                                                                         |                           |

## Command 0x0B - Battery

| Command | Subcommand | Usage | Example Request | Example Response |
| ---     | ---        | ---   | ---             | ---              |
| `0x0B`  | `0x03`     |       |                 |                  |
| `0x0B`  | `0x04`     |       |                 |                  |
| `0x0B`  | `0x06`     |       |                 |                  |
| `0x0B`  | `0x07`     |       |                 |                  |

## Command 0x0C - Feature Select

| Command | Subcommand | Usage                   | Example Request                         | Example Response                        |
| ---     | ---        | ---                     | ---                                     | ---                                     |
| `0x0C`  | `0x01`     |                         |                                         |                                         |
| `0x0C`  | `0x02`     | Init feature flags?     | `0c 91 01 02 00 04 00 00` `2f 00 00 00` | `0c 01 01 02 10 78 00 00` `00 00 00 00` |
| `0x0C`  | `0x03`     |                         |                                         |                                         |
| `0x0C`  | `0x04`     | Confirm feature flags?  | `0c 91 01 04 00 04 00 00` `2f 00 00 00` | `0c 01 01 04 10 78 00 00` `00 00 00 00` |
| `0x0C`  | `0x05`     |                         |                                         |                                         |

## Command 0x0D - Firmware Update

| Command | Subcommand | Usage   | Example Request                                                                                                                                                                                                                                                                                                                         | Example Response          |
| ---     | ---        | ---     | ---                                                                                                                                                                                                                                                                                                                                     | ---                       |
| `0x0D`  | `0x01`     | Unknown | `0d 91 01 01 00 00 00 00`                                                                                                                                                                                                                                                                                                               | `0d 01 01 01 10 78 00 00` |
| `0x0D`  | `0x02`     | Unknown | `0d 91 01 02 00 05 00 00` `02 00 50 07 00`                                                                                                                                                                                                                                                                                              | `0d 01 01 02 10 78 00 00` |
| `0x0D`  | `0x03`     | Unknown | `0d 91 01 03 00 09 00 00` `02 00 00 00 00 60 b0 03 00`                                                                                                                                                                                                                                                                                  | `0d 01 01 03 10 78 00 00` |
| `0x0D`  | `0x04`     | Unknown | `0d 91 01 04 00 64 00 00` `60 00 00 00 db 9d e7 fd 8a 3f 7f b1 7e 9b 56 1b dc f2 a5 94 3a 42 b5 2c c9 c6 d2 f6 33 99 4d b6 63 5c 74 41 8b fa 9a 08 b0 94 4b e2 d4 19 07 7d 4b 59 bf 78 1f b0 97 2a 9c 6c e1 a7 a1 c1 0d e2 44 ad 42 4f 20 84 27 bc ee 16 f4 58 19 96 12 a8 fa 4f ef 0d b5 62 a6 5c ed 4d 94 d0 c5 82 39 94 7d d1 14 95` | `0d 01 01 04 10 78 00 00` |
| `0x0D`  | `0x05`     | Unknown | `0d 91 01 05 00 00 00 00`                                                                                                                                                                                                                                                                                                               | `0d 01 01 05 10 78 00 00` |
| `0x0D`  | `0x06`     | Unknown | `0d 91 01 06 00 0d 00 00` `02 00 00 00 00 60 b0 03 00 04 d5 54 f0`                                                                                                                                                                                                                                                                      | `0d 01 01 06 10 78 00 00` |
| `0x0D`  | `0x07`     | Unknown | `0d 91 01 07 00 00 00 00`                                                                                                                                                                                                                                                                                                               | `0d 01 01 07 10 78 00 00` |

## Command 0x0E - Unknown

## Command 0x0F - Unknown

| Command | Subcommand | Usage | Example Request | Example Response |
| ---     | ---        | ---   | ---             | ---              |
| `0x0F`  | `0x04`     |       |                 |                  |

## Command 0x10 - Unknown

| Command | Subcommand | Usage   | Example Request           | Example Response                                                |
| ---     | ---        | ---     | ---                       | ---                                                             |
| `0x10`  | `0x01`     | Unknown | `10 91 01 01 00 00 00 00` | `10 01 01 01 10 78 00 00` `01 00 0e 01 0c 00 00 00 ff ff ff ff` |

## Command 0x11 - Unknown

| Command | Subcommand | Usage   | Example Request           | Example Response                                                                                                   |
| ---     | ---        | ---     | ---                       | ---                                                                                                                |
| `0x11`  | `0x01`     | Unknown | `11 91 01 01 00 00 00 00` | `11 01 01 01 10 78 00 00` `01 00 00 00`                                                                            |
| `0x11`  | `0x03`     | Unknown | `11 91 01 03 00 00 00 00` | `11 01 01 03 10 78 00 00` `01 20 03 00 00 0a e8 1c 3b 79 7d 8b 3a 0a e8 9c 42 58 a0 0b 42 0a e8 9c 41 58 a0 0b 41` |
| `0x11`  | `0x04`     |         |                           |                                                                                                                    |

## Command 0x12 - Unknown

## Command 0x13 - Unknown

## Command 0x14 - Unknown

| Command | Subcommand | Usage | Example Request | Example Response |
| ---     | ---        | ---   | ---             | ---              |
| `0x14`  | `0x01`     |       |                 |                  |

## Command 0x15 - Pairing

| Command | Subcommand | Usage                          | Example Request                                                                | Example Response                                                               |
| ---     | ---        | ---                            | ---                                                                            | ---                                                                            |
| `0x15`  | `0x01`     | Exchange adapter address(es)   | `15 91 01 01 00 0e 00 00` `00 02 81 eb 3a eb f1 48 80 eb 3a eb f1 48`          | `15 01 01 01 10 78 00 00` `01 04 01 88 16 c2 55 e2 98`                         |
| `0x15`  | `0x02`     | Confirm LTK challenge/response | `15 91 01 02 00 11 00 00` `00 6f c6 df 8a d8 fe df 15 bb 8c 15 e9 1f 32 05 44` | `15 01 01 02 10 78 00 00` `01 13 4c 97 f5 11 b9 b6 dd 4d 86 fd 40 f5 36 e9 ed` |
| `0x15`  | `0x03`     | Finalise pairing               | `15 91 01 03 00 01 00 00` `00`                                                 | `15 01 01 03 10 78 00 00` `01`                                                 |
| `0x15`  | `0x04`     | Exchange LTK components        | `15 91 01 04 00 11 00 00` `00 35 03 e9 29 82 87 71 24 be a8 0c 66 46 15 83 4b` | `15 01 01 04 10 78 00 00` `01 5c f6 ee 79 2c df 05 e1 ba 2b 63 25 c4 1a 5f 10` |

## Command 0x16 - Unknown

| Command | Subcommand | Usage   | Example Request           | Example Response                                                                                    |
| ---     | ---        | ---     | ---                       | ---                                                                                                 |
| `0x16`  | `0x01`     | Unknown | `16 91 01 01 00 00 00 00` | `16 01 01 01 10 78 00 00` `00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00` |

## Command 0x17 - Unknown

## Command 0x18 - Unknown

| Command | Subcommand | Usage   | Example Request           | Example Response                                    |
| ---     | ---        | ---     | ---                       | ---                                                 |
| `0x18`  | `0x01`     | Unknown | `18 91 01 01 00 00 00 00` | `18 01 01 01 10 78 00 00` `00 00 40 f0 00 00 60 00` |
| `0x18`  | `0x02`     |         |                           |                                                     |
| `0x18`  | `0x03`     |         |                           |                                                     |
| `0x18`  | `0x04`     |         |                           |                                                     |
