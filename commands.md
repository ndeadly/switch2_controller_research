# Commands

Most commands listed below with example data have been observed in use by the console through analysing sniffed USB or Bluetooth data. Other commands have been detected by fuzzing the command endpoints and checking for a success response. These commands may or may not be used in practice, and may not even be fully implemented in controller firmware (many return all zero).

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

| Command ID                                | Usage             |
| ---                                       | ---               |
| [0x01](#command-0x01---nfc)               | NFC               |
| [0x02](#command-0x02---flash-memory)      | Flash Memory      |
| [0x03](#command-0x03---initialisation)    | Initialisation    |
| [0x04](#command-0x04---unknown)           | Unknown           |
| [0x05](#command-0x05---unknown)           | Unknown           |
| [0x06](#command-0x06---unknown)           | Unknown           |
| [0x07](#command-0x07---unknown)           | Unknown           |
| [0x08](#command-0x08---charging-grip)     | Charging Grip     |
| [0x09](#command-0x09---player-leds)       | Player LEDs       |
| [0x0A](#command-0x0a---vibration)         | Vibration         |
| [0x0B](#command-0x0b---battery)           | Battery           |
| [0x0C](#command-0x0c---feature-select)    | Feature Select    |
| [0x0D](#command-0x0d---firmware-update)   | Firmware Update   |
| [0x0E](#command-0x0e---unknown)           | Unknown           |
| [0x0F](#command-0x0f---unknown)           | Unknown           |
| [0x10](#command-0x10---firmware-info)     | Firmware Info     |
| [0x11](#command-0x11---unknown)           | Unknown           |
| [0x12](#command-0x12---unknown)           | Unknown           |
| [0x13](#command-0x13---unknown)           | Unknown           |
| [0x14](#command-0x14---unknown)           | Unknown           |
| [0x15](#command-0x15---bluetooth-pairing) | Bluetooth Pairing |
| [0x16](#command-0x16---unknown)           | Unknown           |
| [0x17](#command-0x17---unknown)           | Unknown           |
| [0x18](#command-0x18---unknown)           | Unknown           |

---

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
| `0x01`  | `0x14`     |       |                           |                                         |


### Subcommand 0x0C - Unknown

Unknown.

**Request data:**

*None*

**Response data:** 

| Offset | Size | Value   | Comment |
| ---    | ---  | ---     | ---     |
| 0x0    | 0x4  | Unknown |         |

---

## Command 0x02 - Flash Memory

| Command | Subcommand                                       | Usage               | Example Request                                                                                                                                                                                                                                     | Example Response                                                                                                                                                                                                                                    |
| ---     | ---                                              | ---                 | ---                                                                                                                                                                                                                                                 | ---                                                                                                                                                                                                                                                 |
| `0x02`  | [`0x01`](#subcommand-0x01---read-memory-block)   | Read memory block   | `02 91 01 01 00 08 00 00` `00 00 00 00 00 50 17 00`                                                                                                                                                                                                 | `02 01 01 01 10 78 00 00` `40 00 00 00 00 50 17 00 44 53 50 48 01 00 00 00 a0 96 02 00 00 02 02 00 00 00 09 10 fd 16 29 8a 06 5f 04 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00` |
| `0x02`  | [`0x02`](#subcommand-0x02---write-memory-block)  | Write memory block  | `02 91 01 02 00 48 00 00` `00 00 00 00 00 c0 1f 00 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69 69` | `02 01 01 02 10 78 00 00` `00 00 00 00 00 c0 1f 00`                                                                                                                                                                                                 |
| `0x02`  | [`0x03`](#subcommand-0x03---erase-memory-sector) | Erase memory sector | `02 91 01 03 00 08 00 00` `00 00 00 00 00 c0 1f 00`                                                                                                                                                                                                 | `02 01 01 03 00 78 00 00` `00 00 00 00`                                                                                                                                                                                                             |
| `0x02`  | [`0x04`](#subcommand-0x04---memory-read)         | Memory read         | `02 91 01 04 00 08 00 00` `10 7e 00 00 40 30 01 00`                                                                                                                                                                                                 | `02 01 01 04 10 78 00 00` `10 00 00 00 40 30 01 00 3b e0 d3 41 c6 60 6a bc 4d d7 a2 bb 71 1e dd 37`                                                                                                                                                 |
| `0x02`  | [`0x05`](#subcommand-0x05---memory-write)        | Memory write        | `02 91 01 05 00 48 00 00` `40 7e 00 00 00 c0 1f 00 b2 a1 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00` | `02 01 01 05 10 78 00 00` `00 00 00 00 00 c0 1f 00`                                                                                                                                                                                                 |
| `0x02`  | `0x06`                                           | Unknown             |                                                                                                                                                                                                                                                     |                                                                                                                                                                                                                                                     |


### Subcommand 0x01 - Read Memory Block

Reads a `0x40` byte block of internal flash [memory](memory_layout.md#memory-layout).

**Request data:**

| Offset | Size | Value        | Comment                              |
| ---    | ---  | ---          | ---                                  |
| 0x0    | 0x4  | Unknown      | All `0x00`                           |
| 0x4    | 0x4  | Read address | Address to read from (Little-Endian) |

**Response data:** 

| Offset | Size | Value        | Comment                                  |
| ---    | ---  | ---          | ---                                      |
| 0x0    | 0x1  | Read length  | Number of bytes read. Always `0x40`      |
| 0x1    | 0x3  | Unknown      | All `0x00`. Probably padding             |
| 0x4    | 0x4  | Read address | Where data was read from (Little-Endian) |
| 0x8    | 0x40 | Data         | Data read from the given address         |


### Subcommand 0x02 - Write Memory Block

Writes a `0x40` byte block of data to internal flash [memory](memory_layout.md#memory-layout).

**Request data:**

| Offset | Size | Value        | Comment                             |
| ---    | ---  | ---          | ---                                 |
| 0x0    | 0x4  | Unknown      | All `0x00`                          |
| 0x4    | 0x4  | Read address | Address to write to (Little-Endian) |
| 0x8    | 0x40 | Data         | Data to write                       |

**Response data:** 

| Offset | Size | Value        | Comment                                  |
| ---    | ---  | ---          | ---                                      |
| 0x0    | 0x1  | Read length  | Number of bytes read. Always `0x40`      |
| 0x1    | 0x3  | Unknown      | All `0x00`. Probably padding             |
| 0x4    | 0x4  | Read address | Where data was read from (Little-Endian) |
| 0x8    | 0x40 | Data         | Data read from the given address         |


### Subcommand 0x03 - Erase Memory Sector

Erases a `0x1000` byte memory sector, setting all bytes to `0xFF`.

**Request data:**

| Offset | Size | Value   | Comment                                            |
| ---    | ---  | ---     | ---                                                |
| 0x0    | 0x4  | Unknown | All `0x00`                                         |
| 0x4    | 0x4  | Address | Address within the sector to erase (Little-Endian) |

**Response data:** 

| Offset | Size | Value   | Comment    |
| ---    | ---  | ---     | ---        |
| 0x0    | 0x4  | Unknown | All `0x00` |


### Subcommand 0x04 - Memory Read

Read from internal flash [memory](memory_layout.md#memory-layout). Maximum read length is `0x50` bytes for USB, and `0x4F`for Bluetooth. Addresses above 0x200000 are wrapped around to zero.

**Request data:**

| Offset | Size | Value        | Comment                              |
| ---    | ---  | ---          | ---                                  |
| 0x0    | 0x1  | Read length  | Number of bytes to read              |
| 0x1    | 0x1  | Unknown      | Always `0x7E`                        |
| 0x2    | 0x2  | Unknown      | All `0x00`. Maybe padding            |
| 0x4    | 0x4  | Read address | Address to read from (Little-Endian) |

**Response data:** 

| Offset | Size   | Value        | Comment                                  |
| ---    | ---    | ---          | ---                                      |
| 0x0    | 0x1    | Read length  | Number of bytes read                     |
| 0x1    | 0x3    | Unknown      | All `0x00`. Probably padding             |
| 0x4    | 0x4    | Read address | Where data was read from (Little-Endian) |
| 0x8    | 0-0x50 | Data         | Data read from the given address         |

### Subcommand 0x05 - Memory Write

Write to internal flash [memory](memory_layout.md#memory-layout). Maximum write length is `0x80` bytes. Only addresses >= `0x1F5000` have write permissions for this command. Attempting to write addresses below this will return status `0x81` in the response.

**Request data:**

| Offset | Size     | Value         | Comment                             |
| ---    | ---      | ---           | ---                                 |
| 0x0    | 0x1      | Write Length  | Number of bytes to write            |
| 0x1    | 0x1      | Unknown       | Always `0x7E`                       |
| 0x2    | 0x2      | Unknown       | All `0x00`. Maybe padding           |
| 0x4    | 0x4      | Write address | Address to write to (Little-Endian) |
| 0x8    | 0x0-0x4F | Data          | Data to write                       |

**Response data:** 

| Offset | Size | Value         | Comment                                   |
| ---    | ---  | ---           | ---                                       |
| 0x0    | 0x4  | Unknown       | All `0x00`                                |
| 0x4    | 0x4  | Write address | Where data was written to (Little-Endian) |

---

## Command 0x03 - Initialisation

| Command | Subcommand                                       | Usage               | Example Request                                                                               | Example Response                        |
| ---     | ---                                              | ---                 | ---                                                                                           | ---                                     |
| `0x03`  | `0x01`                                           |                     |                                                                                               |                                         |
| `0x03`  | `0x02`                                           |                     |                                                                                               |                                         |
| `0x03`  | `0x03`                                           |                     |                                                                                               |                                         |
| `0x03`  | `0x04`                                           |                     |                                                                                               |                                         |
| `0x03`  | `0x05`                                           |                     |                                                                                               |                                         |
| `0x03`  | `0x06`                                           |                     |                                                                                               |                                         |
| `0x03`  | [`0x07`](#subcommand-0x07---store-pairing-info)  | Store pairing info  | `03 91 01 07 00 16 00 00` `5e 11 85 eb f1 48 c1 27 80 67 1a fd 29 b8 00 e1 dd c5 19 b4 f0 54` | `03 01 01 07 10 78 00 00`               |
| `0x03`  | `0x08`                                           |                     |                                                                                               |                                         |
| `0x03`  | `0x09`                                           | Unknown             | `03 91 01 09 00 00 00 00`                                                                     | `03 01 01 09 10 78 00 00`               |
| `0x03`  | [`0x0A`](#subcommand-0x0a---select-input-report) | Select input report | `03 91 00 0a 00 04 00 00` `09 00 00 00`                                                       | `03 01 00 0a 00 f8 00 00`               |
| `0x03`  | `0x0C`                                           | Unknown             | `03 91 00 0c 00 04 00 00` `01 00 00 00`                                                       | `03 01 00 0c 00 f8 00 00`               |
| `0x03`  | `0x0D`                                           | Initialise USB      | `03 91 00 0d 00 08 00 00` `01 00 31 7e c6 eb f1 48`                                           | `03 01 00 0d 00 f8 00 00` `01 00 00 00` |
| `0x03`  | `0x0F`                                           |                     |                                                                                               |                                         |


### Subcommand 0x07 - Store Pairing Info

Seems to allow for bypassing the [0x15 commands](#command-0x15---bluetooth-pairing) and writes a Bluetooth host address and Long-Term-Key (LTK) to the controller directly.

**Request data:**

| Offset | Size | Value        | Comment                                       |
| ---    | ---  | ---          | ---                                           |
| 0x0    | 0x6  | Host address | Bluetooth address of the host (byte-reversed) |
| 0x6    | 0x10 | LTK          | Bluetooth LTK                 (byte-reversed) |

**Response data:** 

*None*


### Subcommand 0x09 - Unknown

Unknown.

**Request data:**

*None*

**Response data:** 

*None*


### Subcommand 0x0A - Select Input Report

Selects one of the input report formats found in the HID report descriptor. Invalid report IDs are ignored. Defaults to [`0x07`](hid_reports.md#input-report-0x07)|[`0x08`](hid_reports.md#input-report-0x08)|[`0x09`](hid_reports.md#input-report-0x09)|[`0x0A`](hid_reports.md#input-report-0x0a) (depending on controller type) on power-up.

**Request data:**

| Offset | Size | Value     | Comment                                                                                                                                                                                                                                              |
| ---    | ---  | ---       | ---                                                                                                                                                                                                                                                  |
| 0x0    | 0x1  | Report ID | Input report ID. Either [`0x05`](hid_reports.md#input-report-0x05) or [`0x07`](hid_reports.md#input-report-0x07)\|[`0x08`](hid_reports.md#input-report-0x08)\|[`0x09`](hid_reports.md#input-report-0x09)\|[`0x0A`](hid_reports.md#input-report-0x0a) |
| 0x1    | 0x3  | Unknown   | Seems to be unused                                                                                                                                                                                                                                   |

**Response data:** 

*None*


### Subcommand 0x0C - Unknown

Unknown.

**Request data:**

| Offset | Size | Value   | Comment |
| ---    | ---  | ---     | ---     |
| 0x0    | 0x4  | Unknown |         |

**Response data:** 

*None*


### Subcommand 0x0D - Initialise USB

Initialise the controller for USB. Required before the controller will send input reports over USB. Host address doesn't need to be valid.

**Request data:**

| Offset | Size | Value        | Comment                                       |
| ---    | ---  | ---          | ---                                           |
| 0x0    | 0x1  | Unknown      |                                               |
| 0x1    | 0x1  | Unknown      |                                               |
| 0x2    | 0x6  | Host address | Bluetooth address of the host (byte-reversed) |

**Response data:** 

| Offset | Size | Value   | Comment |
| ---    | ---  | ---     | ---     |
| 0x0    | 0x4  | Unknown |         |


---

## Command 0x04 - Unknown

*Possibly unused*

---

## Command 0x05 - Unknown

| Command | Subcommand | Usage   | Example Request | Example Response |
| ---     | ---        | ---     | ---             | ---              |
| `0x05`  | `0x01`     |         |                 |                  |

---

## Command 0x06 - Unknown

| Command | Subcommand                                     | Usage              | Example Request                         | Example Response          |
| ---     | ---                                            | ---                | ---                                     | ---                       |
| `0x06`  | `0x01`                                         |                    |                                         |                           |
| `0x06`  | `0x02`                                         |                    |                                         |                           |
| `0x06`  | [`0x03`](#subcommand-0x03---reboot-controller) | Reboot controller? | `06 91 01 03 00 04 00 00` `01 00 00 00` | `06 01 01 03 10 78 00 00` |


### Subcommand 0x03 - Reboot Controller?

This command is called following a controller firmware update and appears to reboot the controller and/or reload the firmware.

**Request data:**

| Offset | Size | Value   | Comment               |
| ---    | ---  | ---     | ---                   |
| 0x0    | 0x1  | Unknown | Maybe a boolean flag? |
| 0x1    | 0x3  | Unknown | Seems to be unused    |

**Response data:** 

*None*

---

## Command 0x07 - Unknown

| Command | Subcommand | Usage   | Example Request           | Example Response               |
| ---     | ---        | ---     | ---                       | ---                            |
| `0x07`  | `0x01`     | Unknown | `07 91 01 01 00 00 00 00` | `07 01 01 01 10 78 00 00` `00` |
| `0x07`  | `0x02`     |         |                           |                                |


### Subcommand 0x01 - Unknown

Unknown. This is the first command sent during initialisation.

**Request data:**

*None*

**Response data:** 

| Offset | Size | Value   | Comment               |
| ---    | ---  | ---     | ---                   |
| 0x0    | 0x1  | Unknown | Maybe a boolean flag? |

---

## Command 0x08 - Charging Grip

| Command | Subcommand                                                     | Usage                               | Example Request                         | Example Response                                                                                                                                                                                                                        |
| ---     | ---                                                            | ---                                 | ---                                     | ---                                                                                                                                                                                                                                     |
| `0x08`  | [`0x01`](#subcommand-0x01---get-charging-grip-info-0x20-bytes) | Get charging grip info (0x20 bytes) | `08 91 00 01 00 04 00 00` `20 00 00 00` | `08 01 00 01 00 f8 00 00` `00 00 00 00 01 00 48 44 4c 35 30 30 30 33 34 38 35 35 31 39 00 00 7e 05 68 20 01 03 01 ff ff ff ff ff ff ff`                                                                                                 |
| `0x08`  | [`0x02`](#subcommand-0x02---enable-charging-grip-buttons)      | Enable charging grip buttons        | `08 91 00 02 00 04 00 00` `01 00 00 00` | `08 01 00 02 00 f8 00 00`                                                                                                                                                                                                               |
| `0x08`  | [`0x03`](#subcommand-0x03---get-charging-grip-info-0x40-bytes) | Get charging grip info (0x40 bytes) | `08 91 00 03 00 04 00 00` `40 00 00 00` | `08 01 00 03 00 f8 00 00` `00 00 00 00 01 00 48 44 4c 35 30 30 30 33 34 38 35 35 31 39 00 00 7e 05 68 20 01 03 01 ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff` |


### Subcommand 0x01 - Get Charging Grip Info (0x20 Bytes)

Reads up to 0x20 bytes of data from the charging grip. Data is similar to the factory data found at offset [`0x13000`](memory_layout.md#0x13000-0x14fff) in controller memory. Response always contains 0x20 bytes, unused bytes are initialised to 0.

**Request data:**

| Offset | Size | Value   | Comment                 |
| ---    | ---  | ---     | ---                     |
| 0x0    | 0x1  | Size    | Number of bytes to read |
| 0x1    | 0x3  | Unknown | Seems to be unused      |

**Response data:**

| Offset | Size | Value   | Comment                |
| ---    | ---  | ---     | ---                    |
| 0x0    | 0x4  | Unknown | Always 0. Maybe offset |
| 0x4    | 0x20 | Data    | Charging grip data     |


### Subcommand 0x02 - Enable Charging Grip Buttons

Enables the GL/GR buttons on the charging grip.

**Request data:**

| Offset | Size | Value   | Comment                                      |
| ---    | ---  | ---     | ---                                          |
| 0x0    | 0x1  | Enable  | Boolean value to enable/disable grip buttons |
| 0x1    | 0x3  | Unknown | Seems to be unused                           |

**Response data:**

*None*


### Subcommand 0x03 - Get Charging Grip Info (0x40 Bytes)

Same as subcommand [0x01](#subcommand-0x01---get-charging-grip-info-0x20-bytes), only returns 0x40 bytes instead of 0x20.

**Request data:**

| Offset | Size | Value   | Comment                 |
| ---    | ---  | ---     | ---                     |
| 0x0    | 0x1  | Size    | Number of bytes to read |
| 0x1    | 0x3  | Unknown | Seems to be unused      |

**Response data:**

| Offset | Size | Value   | Comment                |
| ---    | ---  | ---     | ---                    |
| 0x0    | 0x4  | Unknown | Always 0. Maybe offset |
| 0x4    | 0x40 | Data    | Charging grip data     |

---

## Command 0x09 - Player LEDs

| Command | Subcommand                                    | Usage            | Example Request                                     | Example Response          |
| ---     | ---                                           | ---              | ---                                                 | ---                       |
| `0x09`  | [`0x01`](#subcommand-0x01---set-player-1-led) | Set player 1 LED | `09 91 00 01 00 00 00 00`                           | `09 01 00 01 10 78 00 00` |
| `0x09`  | [`0x02`](#subcommand-0x02---set-player-2-led) | Set player 2 LED | `09 91 00 02 00 00 00 00`                           | `09 01 00 02 10 78 00 00` |
| `0x09`  | [`0x03`](#subcommand-0x03---set-player-3-led) | Set player 3 LED | `09 91 00 03 00 00 00 00`                           | `09 01 00 03 10 78 00 00` |
| `0x09`  | [`0x04`](#subcommand-0x04---set-player-4-led) | Set player 4 LED | `09 91 00 04 00 00 00 00`                           | `09 01 00 04 10 78 00 00` |
| `0x09`  | [`0x05`](#subcommand-0x05---set-all-leds-on)  | Set all LEDs on  | `09 91 00 05 00 00 00 00`                           | `09 01 00 05 10 78 00 00` |
| `0x09`  | [`0x06`](#subcommand-0x06---set-all-leds-off) | Set all LEDs off | `09 91 00 06 00 00 00 00`                           | `09 01 00 06 10 78 00 00` |
| `0x09`  | [`0x07`](#subcommand-0x07---set-led-pattern)  | Set LED pattern  | `09 91 01 07 00 08 00 00` `01 00 00 00 00 00 00 00` | `09 01 01 07 10 78 00 00` |
| `0x09`  | [`0x08`](#subcommand-0x08---flash-leds)       | Flash LEDs       | `09 91 01 08 00 04 00 00` `01 00 00 00`             | `09 01 01 08 10 78 00 00` |


### Subcommand 0x01 - Set Player 1 LED

Sets player 1 LED on and disables all others. Equivalent to subcommand `0x07` with argument `0b0001`.

**Request data:**

*None*

**Response data:** 

*None*


### Subcommand 0x02 - Set Player 2 LED

Sets player 2 LED on and disables all others. Equivalent to subcommand `0x07` with argument `0b0010`.

**Request data:**

*None*

**Response data:** 

*None*


### Subcommand 0x03 - Set Player 3 LED

Sets player 3 LED on and disables all others. Equivalent to subcommand `0x07` with argument `0b0100`.

**Request data:**

*None*

**Response data:** 

*None*


### Subcommand 0x04 - Set Player 4 LED

Sets player 4 LED on and disables all others. Equivalent to subcommand `0x07` with argument `0b1000`.

**Request data:**

*None*

**Response data:** 

*None*


### Subcommand 0x05 - Set All LEDs On

Sets all player LEDs on. Equivalent to subcommand `0x07` with argument `0b1111`.

**Request data:**

*None*

**Response data:** 

*None*


### Subcommand 0x06 - Set All LEDs Off

Sets all player LEDs off. Equivalent to subcommand `0x07` with argument `0b0000`.

**Request data:**

*None*

**Response data:** 

*None*


### Subcommand 0x07 - Set LED Pattern

Set the player LED pattern on the controller.

**Request data:**

| Offset | Size | Value       | Comment                                          |
| ---    | ---  | ---         | ---                                              |
| 0x0    | 0x1  | LED bitmask | Bits 0-3 correspond to each of the 4 player LEDS |
| 0x1    | 0x7  | Unknown     | Seems to be unused                               |

**Response data:** 

*None*


### Subcommand 0x08 - Flash LEDs

Set the current player LED pattern to flash on and off.

**Request data:**

| Offset | Size | Value   | Comment                                      |
| ---    | ---  | ---     | ---                                          |
| 0x0    | 0x1  | Enable  | Boolean value to enable/disable LED flashing |
| 0x1    | 0x7  | Unknown | Seems to be unused                           |

**Response data:** 

*None*


---

## Command 0x0A - Vibration

| Command | Subcommand                                         | Usage                 | Example Request                                                                         | Example Response          |
| ---     | ---                                                | ---                   | ---                                                                                     | ---                       |
| `0x0A`  | `0x01`                                             |                       |                                                                                         |                           |
| `0x0A`  | [`0x02`](#subcommand-0x02---play-vibration-sample) | Play vibration sample | `0a 91 01 02 00 04 00 00` `03 00 00 00`                                                 | `0a 01 01 02 10 78 00 00` |
| `0x0A`  | `0x03`                                             |                       |                                                                                         |                           |
| `0x0A`  | `0x04`                                             |                       |                                                                                         |                           |
| `0x0A`  | `0x05`                                             |                       |                                                                                         |                           |
| `0x0A`  | `0x06`                                             |                       |                                                                                         |                           |
| `0x0A`  | `0x07`                                             |                       |                                                                                         |                           |
| `0x0A`  | [`0x08`](#subcommand-0x08---send-vibration-data)   | Send vibration data   | `0a 91 01 08 00 14 00 00` `01 ff ff ff ff ff ff ff ff 35 00 46 00 00 00 00 00 00 00 00` | `0a 01 01 08 10 78 00 00` |
| `0x0A`  | `0x09`                                             |                       |                                                                                         |                           |


### Subcommand 0x02 - Play Vibration Sample

Plays a short predefined vibration sample identified by ID.

| Sample ID | Description                                            | Official Use                  |
| ---       | ---                                                    | ---                           |
| 0x1       | Low frequency buzz, ~1s                                |                               |
| 0x2       | High frequency buzz, followed by beep-beep alarm sound | "Search For Controllers" tone |
| 0x3       | Soft click-click vibration                             | Connection                    |
| 0x4       | High to higher-frequency beep-beep sound               | Low battery?                  |
| 0x5       | Like 0x03, but stronger                                |                               |
| 0x6       | Short, high-frequency beep                             |                               |
| 0x7       | Short, higher-frequency beep                           |                               |

**Request data:**

| Offset | Size | Value     | Comment              |
| ---    | ---  | ---       | ---                  |
| 0x0    | 0x1  | Sample ID | ID of sample to play |
| 0x1    | 0x3  | Unknown   | Seems to be unused   |

**Response data:** 

*None*


### Subcommand 0x08 - Send Vibration Data

Sends raw vibration data/commands to the controller.

**Request data:**

| Offset | Size | Value          | Comment                         |
| ---    | ---  | ---            | ---                             |
| 0x0    | 0x14 | Vibration data | Format not yet fully understood |

**Response data:** 

*None*

---

## Command 0x0B - Battery

| Command | Subcommand                                       | Usage               | Example Request           | Example Response                        |
| ---     | ---                                              | ---                 | ---                       | ---                                     |
| `0x0B`  | [`0x03`](#subcommand-0x03---get-battery-voltage) | Get battery voltage | `0b 91 00 03 00 00 00 00` | `0b 01 00 03 10 78 00 00` `a5 0e 00 00` |
| `0x0B`  | [`0x04`](#subcommand-0x04---get-charge-status)   | Get charge status   | `0b 91 00 04 00 00 00 00` | `0b 01 00 04 10 78 00 00` `34 00 83 00` |
| `0x0B`  | `0x06`                                           | Unknown             | `0b 91 00 06 00 00 00 00` | `0b 01 00 06 10 78 00 00` `11 00 00 00` |
| `0x0B`  | `0x07`                                           |                     |                           |                                         |


### Subcommand 0x03 - Get Battery Voltage

Returns the current battery voltage in mV.

**Request data:**

*None*

**Response data:** 

| Offset | Size | Value   | Comment                                                                                                                         |
| ---    | ---  | ---     | ---                                                                                                                             |
| 0x0    | 0x2  | Voltage | Battery voltage in mV. This is the same value found in input report [`0x05`](hid_reports.md#input-report-0x05) at offset `0x1F` |
| 0x2    | 0x2  | Unknown | Possibly padding                                                                                                                |


### Subcommand 0x04 - Get Charge Status

Returns the charging status and what seems to be some flags.

**Request data:**

*None*

**Response data:** 

| Offset | Size | Value         | Comment                                                                                                                   |
| ---    | ---  | ---           | ---                                                                                                                       |
| 0x0    | 0x2  | Charge status | Charging status. This is the same value found in input report [`0x05`](hid_reports.md#input-report-0x05) at offset `0x21` |
| 0x2    | 0x2  | Unknown       | Seems to be a set of flags?                                                                                               |


### Subcommand 0x06 - Unknown

Unknown.

**Request data:**

*None*

**Response data:** 

| Offset | Size | Value   | Comment                         |
| ---    | ---  | ---     | ---                             |
| 0x0    | 0x4  | Unknown | Seems to be always `0x00000011` |

---

## Command 0x0C - Feature Select

These commands are used for configuring which features are enabled in the HID reports sent by the controller.

| Command | Subcommand                                      | Usage               | Example Request                         | Example Response                                                |
| ---     | ---                                             | ---                 | ---                                     | ---                                                             |
| `0x0C`  | [`0x01`](#subcommand-0x01---get-feature-info)   | Get feature info    | `0c 91 01 01 00 04 00 00` `2f 00 00 00` | `0c 01 01 01 10 78 00 00` `00 00 00 00 07 07 01 00 00 03 00 00` |
| `0x0C`  | [`0x02`](#subcommand-0x02---set-feature-mask)   | Set feature mask    | `0c 91 01 02 00 04 00 00` `2f 00 00 00` | `0c 01 01 02 10 78 00 00` `00 00 00 00`                         |
| `0x0C`  | [`0x03`](#subcommand-0x03---clear-feature-mask) | Clear feature mask  | `0c 91 01 03 00 04 00 00` `2f 00 00 00` | `0c 01 01 03 10 78 00 00` `00 00 00 00`                         |
| `0x0C`  | [`0x04`](#subcommand-0x04---enable-features)    | Enable features     | `0c 91 01 04 00 04 00 00` `2f 00 00 00` | `0c 01 01 04 10 78 00 00` `00 00 00 00`                         |
| `0x0C`  | [`0x05`](#subcommand-0x05---disable-features)   | Disable features    | `0c 91 01 05 00 04 00 00` `10 00 00 00` | `0c 01 01 05 10 78 00 00` `00 00 00 00`                         |

##### Feature Flags:

| Bit | Mask   | Feature       | Comment                   |
| --- | ---    | ---           | ---                       |
| 0   | `0x01` | Button state  |                           |
| 1   | `0x02` | Analog sticks |                           |
| 2   | `0x04` | IMU           | Linear accelometer + gyro |
| 3   | `0x08` | -             | Unused                    |
| 4   | `0x10` | Mouse data    | JoyCon only               |
| 5   | `0x20` | Current       | Seems to be JoyCon only   |
| 6   | `0x40` | -             | Unused                    |
| 7   | `0x80` | Magnetometer  |                           |


### Subcommand 0x01 - Get Feature Info

Returns 8 bytes of unknown data corresponding to the passed feature flags. Each byte position in the output corresponds to the bit index of a feature apart from byte 3, where the output for bit 7 has been moved to fill the gap. Bytes can take on the values `0x00`, `0x01`, `0x03` or `0x07`. The last 2 bytes are unused.

The output mapping can be described by the following python function:

```python
def encode_output(flags):
    out = bytearray(8)
    out[0] = 0x07 if flags & 0x01 else 0x00
    out[1] = 0x07 if flags & 0x02 else 0x00
    out[2] = 0x01 if flags & 0x04 else 0x00  # 0x03 for JoyCon
    out[3] = 0x01 if flags & 0x80 else 0x00  # 0x03 for JoyCon
    out[4] = 0x01 if flags & 0x10 else 0x00  # 0x03 for JoyCon
    out[5] = 0x03 if flags & 0x20 else 0x00
    return out
```

**Request data:**

| Offset | Size | Value   | Comment                         |
| ---    | ---  | ---     | ---                             |
| 0x0    | 0x1  | Flags   | [Feature flags](#feature-flags) |
| 0x1    | 0x3  | Unknown | Seems to be unused              |

**Response data:** 

| Offset | Size | Value        | Comment    |
| ---    | ---  | ---          | ---        |
| 0x0    | 0x4  | Unknown      | All `0x00` |
| 0x4    | 0x8  | Feature info |            |


### Subcommand 0x02 - Set Feature Mask

Sets the feature mask. Feature flags outside of this mask will be ignored by enable/disable commands.  Must be called at least once prior to subcommands [`0x04`](#subcommand-0x04---enable-features) and [`0x05`](#subcommand-0x05---disable-features).

**Request data:**

| Offset | Size | Value   | Comment                         |
| ---    | ---  | ---     | ---                             |
| 0x0    | 0x1  | Flags   | [Feature flags](#feature-flags) |
| 0x1    | 0x3  | Unknown | Seems to be unused              |

**Response data:** 

| Offset | Size | Value   | Comment    |
| ---    | ---  | ---     | ---        |
| 0x0    | 0x4  | Unknown | All `0x00` |


### Subcommand 0x03 - Clear Feature Mask

Clears the feature mask. All report features are disabled until a new mask is set with subcommand `0x02`.

**Request data:**

| Offset | Size | Value   | Comment            |
| ---    | ---  | ---     | ---                |
| 0x0    | 0x4  | Unknown | Seems to be unused |

**Response data:** 

| Offset | Size | Value   | Comment    |
| ---    | ---  | ---     | ---        |
| 0x0    | 0x4  | Unknown | All `0x00` |


### Subcommand 0x04 - Enable Features

Enables the selected features.

**Request data:**

| Offset | Size | Value   | Comment                         |
| ---    | ---  | ---     | ---                             |
| 0x0    | 0x1  | Flags   | [Feature flags](#feature-flags) |
| 0x1    | 0x3  | Unknown | Seems to be unused              |

**Response data:** 

| Offset | Size | Value   | Comment    |
| ---    | ---  | ---     | ---        |
| 0x0    | 0x4  | Unknown | All `0x00` |


### Subcommand 0x05 - Disable Features

Disables the selected features.

**Request data:**

| Offset | Size | Value   | Comment                         |
| ---    | ---  | ---     | ---                             |
| 0x0    | 0x1  | Flags   | [Feature flags](#feature-flags) |
| 0x1    | 0x3  | Unknown | Seems to be unused              |

**Response data:** 

| Offset | Size | Value   | Comment    |
| ---    | ---  | ---     | ---        |
| 0x0    | 0x4  | Unknown | All `0x00` |

---

## Command 0x0D - Firmware Update

| Command | Subcommand                                              | Usage                       | Example Request                                                                                                                                                                                                                                                                                                                         | Example Response          |
| ---     | ---                                                     | ---                         | ---                                                                                                                                                                                                                                                                                                                                     | ---                       |
| `0x0D`  | [`0x01`](#subcommand-0x01---initialise-firmware-update) | Initialise firmware update? | `0d 91 01 01 00 00 00 00`                                                                                                                                                                                                                                                                                                               | `0d 01 01 01 10 78 00 00` |
| `0x0D`  | [`0x02`](#subcommand-0x02---set-failsafe-address)       | Set failsafe address?       | `0d 91 01 02 00 05 00 00` `02 00 50 07 00`                                                                                                                                                                                                                                                                                              | `0d 01 01 02 10 78 00 00` |
| `0x0D`  | [`0x03`](#subcommand-0x03---set-firmware-image-size)    | Set firmware image size?    | `0d 91 01 03 00 09 00 00` `02 00 00 00 00 60 b0 03 00`                                                                                                                                                                                                                                                                                  | `0d 01 01 03 10 78 00 00` |
| `0x0D`  | [`0x04`](#subcommand-0x04---transfer-update-data)       | Transfer update data        | `0d 91 01 04 00 64 00 00` `60 00 00 00 db 9d e7 fd 8a 3f 7f b1 7e 9b 56 1b dc f2 a5 94 3a 42 b5 2c c9 c6 d2 f6 33 99 4d b6 63 5c 74 41 8b fa 9a 08 b0 94 4b e2 d4 19 07 7d 4b 59 bf 78 1f b0 97 2a 9c 6c e1 a7 a1 c1 0d e2 44 ad 42 4f 20 84 27 bc ee 16 f4 58 19 96 12 a8 fa 4f ef 0d b5 62 a6 5c ed 4d 94 d0 c5 82 39 94 7d d1 14 95` | `0d 01 01 04 10 78 00 00` |
| `0x0D`  | [`0x05`](#subcommand-0x05---end-data-transfer)          | End data transfer?          | `0d 91 01 05 00 00 00 00`                                                                                                                                                                                                                                                                                                               | `0d 01 01 05 10 78 00 00` |
| `0x0D`  | [`0x06`](#subcommand-0x06---verify-firmware-update)     | Verify firmware update?     | `0d 91 01 06 00 0d 00 00` `02 00 00 00 00 60 b0 03 00 04 d5 54 f0`                                                                                                                                                                                                                                                                      | `0d 01 01 06 10 78 00 00` |
| `0x0D`  | [`0x07`](#subcommand-0x07---finalise-firmware-update)   | Finalise firmware update?   | `0d 91 01 07 00 00 00 00`                                                                                                                                                                                                                                                                                                               | `0d 01 01 07 10 78 00 00` |


### Subcommand 0x01 - Initialise Firmware Update?

Unknown.

**Request data:**

*None*

**Response data:** 

*None*


### Subcommand 0x02 - Set Failsafe Address?

Unknown.

**Request data:**

| Offset | Size | Value   | Comment                                                                                  |
| ---    | ---  | ---     | ---                                                                                      |
| 0x0    | 0x1  | Unknown | Always `0x02`. Possibly failsafe firmware region ID                                      |
| 0x0    | 0x4  | Address | Address of failsafe firmware region (Little-Endian). Should be either 0x15000 or 0x75000 |

**Response data:** 

*None*


### Subcommand 0x03 - Set Firmware Image Size?

Unknown.

**Request data:**

| Offset | Size | Value   | Comment                                                       |
| ---    | ---  | ---     | ---                                                           |
| 0x0    | 0x1  | Unknown | Always `0x02`. Possibly failsafe firmware region ID           |
| 0x0    | 0x4  | Unknown | All `0x00`. Could be a lower offset from the failsafe address |
| 0x0    | 0x4  | Size    | Firmware update image size in bytes (Little-Endian)           |

**Response data:** 

*None*


### Subcommand 0x04 - Transfer Update Data

Transfers a chunk of update data to the controller.

USB connections call this subcommand repeatedly to transfer blocks of up to 0x4C bytes until all data is transferred.

Bluetooth connections call this subcommand via a special characteristic `4147423d-fdae-4df7-a4f7-d23e5df59f8d` (handle `0x0018`) that allows for spanning commands and their data over multiple writes without responses in between. Data is transferred via this characteristic in blocks of up to 0xB5C bytes, comprised of 30 chunks of up to 0x64 bytes each. At the end of each block, a type 0x04 response is received via the usual command interface. The final chunk of data often doesn't need to span multiple commands and is also transferred using the usual command interface.

**Request data:**

| Offset | Size     | Value   | Comment                       |
| ---    | ---      | ---     | ---                           |
| 0x0    | 0x4      | Length  | Length of the following data  |
| 0x4    | 0x0-0x4C | Data    | The next chunk of update data |

**Response data:** 

*None*


### Subcommand 0x05 - End Data Transfer?

Unknown. Called after last 0x04 subcommand.

**Request data:**

*None*

**Response data:** 

*None*


### Subcommand 0x06 - Verify Firmware Update?

Unknown. Takes the same paramaters as subcommand 0x03 with an additional checksum. Likely for verifying the update was written correctly.

**Request data:**

| Offset | Size | Value    | Comment                                                       |
| ---    | ---  | ---      | ---                                                           |
| 0x0    | 0x1  | Unknown  | Always `0x02`. Possibly failsafe firmware region ID           |
| 0x0    | 0x4  | Unknown  | All `0x00`. Could be a lower offset from the failsafe address |
| 0x0    | 0x4  | Size     | Firmware update image size in bytes (Little-Endian)           |
| 0x0    | 0x4  | Checksum | CRC-32 checksum of the firmware update image (Little-Endian)  |


**Response data:** 

*None*


### Subcommand 0x07 - Finalise Firmware Update?

Unknown. Final firmware update subcommand called before controller is rebooted.

**Request data:**

*None*

**Response data:** 

*None*

---

## Command 0x0E - Unknown

*Seems to be unused*

---

## Command 0x0F - Unknown

Unknown.

| Command | Subcommand | Usage | Example Request | Example Response |
| ---     | ---        | ---   | ---             | ---              |
| `0x0F`  | `0x01`     |       |                 |                  |
| `0x0F`  | `0x02`     |       |                 |                  |
| `0x0F`  | `0x04`     |       |                 |                  |

---

## Command 0x10 - Firmware Info

| Command | Subcommand                                             | Usage                     | Example Request           | Example Response                                                |
| ---     | ---                                                    | ---                       | ---                       | ---                                                             |
| `0x10`  | [`0x01`](#subcommand-0x01---get-firmware-version-info) | Get firmware version info | `10 91 01 01 00 00 00 00` | `10 01 01 01 10 78 00 00` `01 00 0e 01 0c 00 00 00 ff ff ff ff` |


### Subcommand 0x01 - Get Firmware Version Info

Gets version information about the controller firmware.

**Request data:**

*None*

**Response data:** 

| Offset | Size | Value                     | Comment                                                                       |
| ---    | ---  | ---                       | ---                                                                           |
| 0x0    | 0x1  | Controller firmware major | Controller firmware major version number                                      |
| 0x1    | 0x1  | Controller firmware minor | Controller firmware minor version number                                      |
| 0x2    | 0x1  | Controller firmware micro | Controller firmware micro version number                                      |
| 0x3    | 0x1  | Controller firmware type  | `00` = JoyCon (L), `01` = JoyCon (R), `02` = Pro Controller, `03` = Gamecube  |
| 0x4    | 0x4  | Unknown                   | Always `0c 00 00 00`                                                          |
| 0x8    | 0x4  | DSP firmware version      | Only present on Pro Controller with updated firmware, `ff ff ff ff` otherwise |

---

## Command 0x11 - Unknown

| Command | Subcommand | Usage   | Example Request           | Example Response                                                                                                   |
| ---     | ---        | ---     | ---                       | ---                                                                                                                |
| `0x11`  | `0x01`     | Unknown | `11 91 01 01 00 00 00 00` | `11 01 01 01 10 78 00 00` `01 00 00 00`                                                                            |
| `0x11`  | `0x03`     | Unknown | `11 91 01 03 00 00 00 00` | `11 01 01 03 10 78 00 00` `01 20 03 00 00 0a e8 1c 3b 79 7d 8b 3a 0a e8 9c 42 58 a0 0b 42 0a e8 9c 41 58 a0 0b 41` |
| `0x11`  | `0x04`     |         |                           |                                                                                                                    |

---

## Command 0x12 - Unknown

*Seems to be unused*

---

## Command 0x13 - Unknown

Unknown. Seems to only be used on JoyCon controllers.

| Command | Subcommand | Usage   | Example Request           | Example Response                                    |
| ---     | ---        | ---     | ---                       | ---                                                 |
| `0x13`  | `0x01`     | Unknown | `13 91 01 01 00 00 00 00` | `13 01 01 01 10 78 00 00` `01 00 00 00`             |
| `0x13`  | `0x02`     | Unknown | `13 91 01 02 00 00 00 00` | `13 01 01 02 10 78 00 00` `01 00 00 00 00 00 00 00` |
| `0x13`  | `0x03`     | Unknown | `13 91 01 03 00 00 00 00` | `13 01 01 03 10 78 00 00` `01 00 00 00 00 00 00 00` |

---

## Command 0x14 - Unknown

| Command | Subcommand | Usage | Example Request | Example Response |
| ---     | ---        | ---   | ---             | ---              |
| `0x14`  | `0x01`     |       |                 |                  |

---

## Command 0x15 - Bluetooth Pairing

These commands are used for pairing the controller to the host instead of the standard [Security Manager Protocl (SMP)](https://www.bluetooth.com/wp-content/uploads/Files/Specification/HTML/Core-54/out/en/host/security-manager-specification.html#UUID-357f5fc6-5b55-6d33-e459-f42c929137f8) usually used by Bluetooth LE devices. Pairing is required for automatic reconnection and wake-from-sleep functionality to work with the console.

| Command | Subcommand                                      | Usage                          | Example Request                                                                | Example Response                                                               |
| ---     | ---                                             | ---                            | ---                                                                            | ---                                                                            |
| `0x15`  | [`0x01`](#subcommand-0x01---exchange-addresses) | Exchange Bluetooth address(es) | `15 91 01 01 00 0e 00 00` `00 02 81 eb 3a eb f1 48 80 eb 3a eb f1 48`          | `15 01 01 01 10 78 00 00` `01 04 01 88 16 c2 55 e2 98`                         |
| `0x15`  | [`0x02`](#subcommand-0x02---confirm-ltk)        | Confirm LTK challenge/response | `15 91 01 02 00 11 00 00` `00 6f c6 df 8a d8 fe df 15 bb 8c 15 e9 1f 32 05 44` | `15 01 01 02 10 78 00 00` `01 13 4c 97 f5 11 b9 b6 dd 4d 86 fd 40 f5 36 e9 ed` |
| `0x15`  | [`0x03`](#subcommand-0x03---finalise-pairing)   | Finalise pairing               | `15 91 01 03 00 01 00 00` `00`                                                 | `15 01 01 03 10 78 00 00` `01`                                                 |
| `0x15`  | [`0x04`](#subcommand-0x04---exchange-keys)      | Exchange LTK components        | `15 91 01 04 00 11 00 00` `00 35 03 e9 29 82 87 71 24 be a8 0c 66 46 15 83 4b` | `15 01 01 04 10 78 00 00` `01 5c f6 ee 79 2c df 05 e1 ba 2b 63 25 c4 1a 5f 10` |


### Subcommand 0x01 - Exchange Addresses

Exchange Bluetooth adapter addresses between host and controller to be stored alongside the Bluetooth `LTK` for the connection.

**Request data:**

| Offset | Size      | Value        | Comment                                               |
| ---    | ---       | ---          | ---                                                   |
| 0x0    | 0x1       | Unknown      | Always `0x00` for request                             |
| 0x1    | 0x1       | Count        | Number of addresses in the following list             |
| 0x2    | 0x6*Count | Address list | List of host Bluetooth addresses (reverse byte-order) |

**Response data:**

| Offset | Size | Value   | Comment                                                       |
| ---    | ---  | ---     | ---                                                           |
| 0x0    | 0x1  | Unknown | Always `0x01` for response                                    |
| 0x1    | 0x1  | Unknown |                                                               |
| 0x2    | 0x1  | Count?  | Always `0x01`, below could be another list with a single item |
| 0x3    | 0x6  | Address | Controller Bluetooth address (reverse byte-order)             |


### Subcommand 0x02 - Confirm LTK

Send a 16-byte challenge to the controller. The controller computes the response $B2 = \text{AES}^{128}_{LTK}(A2)$. That is, the challenge (`A2`) is encrypted using AES128 in ECB mode with the `LTK` (derived from data exchanged with [subcommand 0x04](#subcommand-0x04---exchange-keys)). Since the challenge/response are transmitted in reverse byte-order, they must be byte-reversed for this computation.

**Request data:**

| Offset | Size | Value     | Comment                                    |
| ---    | ---  | ---       | ---                                        |
| 0x0    | 0x1  | Unknown   | Always `0x00` for request                  |
| 0x1    | 0x10 | Challenge | Host challenge (`A2`) (reverse byte-order) |

**Response data:**

| Offset | Size | Value    | Comment                                     |
| ---    | ---  | ---      | ---                                         |
| 0x0    | 0x1  | Unknown  | Always `0x01` for response                  |
| 0x1    | 0x10 | Response | Device response (`B2`) (reverse byte-order) |


### Subcommand 0x03 - Finalise Pairing

Finalise the pairing procedure and commit the host addresses and Bluetooth LTK to controller memory.

**Request data:**

| Offset | Size | Value   | Comment                   |
| ---    | ---  | ---     | ---                       |
| 0x0    | 0x1  | Unknown | Always `0x00` for request |

**Response data:**

| Offset | Size | Value   | Comment                    |
| ---    | ---  | ---     | ---                        |
| 0x0    | 0x1  | Unknown | Always `0x01` for response |


### Subcommand 0x04 - Exchange Keys

Exchange 16-byte public keys between host and controller. The shared Bluetooth LTK is computed as `A1 ^ B1`.

**Request data:**

| Offset | Size | Value    | Comment                              |
| ---    | ---  | ---      | ---                                  |
| 0x0    | 0x1  | Unknown  | Always `0x00` for request            |
| 0x1    | 0x10 | Host key | Host key (`A1`) (reverse byte-order) |

**Response data:**

| Offset | Size | Value      | Comment                                                                            |
| ---    | ---  | ---        | ---                                                                                |
| 0x0    | 0x1  | Unknown    | Always `0x01` for response                                                         |
| 0x1    | 0x10 | Device key | Device key (`B1`) (reverse byte-order) - always `5CF6EE792CDF05E1BA2B6325C41A5F10` |

---

## Command 0x16 - Unknown

| Command | Subcommand | Usage   | Example Request           | Example Response                                                                                    |
| ---     | ---        | ---     | ---                       | ---                                                                                                 |
| `0x16`  | `0x01`     | Unknown | `16 91 01 01 00 00 00 00` | `16 01 01 01 10 78 00 00` `00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00` |


### Subcommand 0x01 - Unknown

**Request data:**

*None*

**Response data:** 

| Offset | Size | Value    | Comment    |
| ---    | ---  | ---      | ---        |
| 0x0    | 0x18  | Unknown | All `0x00` |

---

## Command 0x17 - Unknown

| Command | Subcommand | Usage   | Example Request                                  | Example Response          |
| ---     | ---        | ---     | ---                                              | ---                       |
| `0x17`  | `0x02`     | Unknown | `17 91 01 02 00 07 00 00` `80 bb 00 00 02 f0 00` | `17 01 01 02 10 78 00 00` |


### Subcommand 0x02 - Unknown

Unknown.

**Request data:**

| Offset | Size | Value   | Comment |
| ---    | ---  | ---     | ---     |
| 0x0    | 0x7  | Unknown |         |

**Response data:** 

*None*

---

## Command 0x18 - Unknown

| Command | Subcommand | Usage   | Example Request                | Example Response                                    |
| ---     | ---        | ---     | ---                            | ---                                                 |
| `0x18`  | `0x01`     | Unknown | `18 91 01 01 00 00 00 00`      | `18 01 01 01 10 78 00 00` `00 00 40 f0 00 00 60 00` |
| `0x18`  | `0x02`     |         |                                |                                                     |
| `0x18`  | `0x03`     | Unknown | `18 91 01 03 00 01 00 00` `07` | `18 01 01 03 10 78 00 00` `07`                      |
| `0x18`  | `0x04`     |         |                                |                                                     |


### Subcommand 0x01 - Unknown

Unknown.

**Request data:**

*None*

**Response data:** 

| Offset | Size | Value   | Comment |
| ---    | ---  | ---     | ---     |
| 0x0    | 0x8  | Unknown |         |

### Subcommand 0x03 - Unknown

Unknown.

**Request data:**

| Offset | Size | Value   | Comment |
| ---    | ---  | ---     | ---     |
| 0x0    | 0x1  | Unknown |         |

**Response data:** 

| Offset | Size | Value   | Comment |
| ---    | ---  | ---     | ---     |
| 0x0    | 0x1  | Unknown |         |

