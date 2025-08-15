
# Memory Layout

Similar to the original Switch controllers, Switch 2 controllers contain nonvolatile (SPI flash?) memory for storing factory data, binary firmware blobs and updates, user calibrations etc. The memory appears to be 2MB in size and can be read or written to via HID commands.

## Overview

The 2MB memory is divided into the following sections. All uninitialised/unused memory is set to `FF`.

| Section                                 | Size    | Usage                      |
| ---                                     | ---     | ---                        |
| [0x0-0x10FFF](#0x0-0x10FFF)             | 0x11000 | Initial FW                 |
| [0x11000-0x11FFF](#0x11000-0x11FFF)     | 0x1000  | Unknown                    |
| [0x12000-0x12FFF](#0x12000-0x12FFF)     | 0x1000  | Unknown                    |
| [0x13000-0x14FFF](#0x13000-0x14FFF)     | 0x2000  | Factory data               |
| [0x15000-0x74FFF](#0x15000-0x74FFF)     | 0x60000 | Failsafe FW update bank #1 |
| [0x75000-0xD4FFF](#0x75000-0xD4FFF)     | 0x60000 | Failsafe FW update bank #2 |
| [0xD5000-0x174FFF](#0xD5000-0x174FFF)   | 0xA0000 | Unknown firmware(?)        |
| [0x175000-0x1F9FFF](#0x175000-0x1F9FFF) | 0x85000 | DSP firmware               |
| [0x1FA000-0x1FAFFF](#0x1FA000-0x1FAFFF) | 0x1000  | Pairing info               |
| [0x1FB000-0x1FBFFF](#0x1FB000-0x1FBFFF) | 0x1000  | Unknown                    |
| [0x1FC000-0x1FCFFF](#0x1FC000-0x1FCFFF) | 0x1000  | User calibration           |
| [0x1FD000-0x1FDFFF](#0x1FD000-0x1FDFFF) | 0x1000  | Shipment?                  |
| [0x1FE000-0x1FEFFF](#0x1FE000-0x1FEFFF) | 0x1000  | Unknown                    |
| [0x1FF000-0x1FFFFF](#0x1FF000-0x1FFFFF) | 0x1000  | Unknown                    |

## 0x0-0x10FFF

Contains the initial firmware/firmware patches. Appears to be encrypted. Header magic [0xAA640001](https://github.com/ARM-software/arm-trusted-firmware/blob/d154fe2bf0616f2e78965207c32b6e83ba12292e/include/tools_share/firmware_encrypted.h#L14) suggests the use of some variant of https://github.com/ARM-software/arm-trusted-firmware, but the header format is different from the official repo.

Structure of the header is as follows:

| Offset | Size   | Example                                                                                           | Usage                                                         |
| ---    | ---    | ---                                                                                               | ---                                                           |
| 0x0    | 0x4    | `01 00 64 AA`                                                                                     | Header magic 0xAA640001                                       |
| 0x4    | 0x4    | `20 53 59 53`                                                                                     | ASCII name "SYS "                                             |
| 0x8    | 0x4    | `C6 38 8F E7`                                                                                     | Unknown (CRC? Flags?)                                         |
| 0xC    | 0x4    | `00 00 77 10`                                                                                     | Size of data including header (u32 BE)                        |
| 0x10   | 0x20   | `83 FF EC 07 46 EC F4 D9 17 2A E5 AE 0B 5D 6B CF 12 4A D5 4C 4D 83 B0 71 BD E7 01 F2 CC B5 8B 45` | Unknown. Unchanged between firmwares. Maybe IV and tag values |

## 0x11000-0x11FFF

Uninitialised on original factory firmware. After update first 4 bytes are initialised to the address where the firmware update is to be loaded from.

| Offset  | Size | Example       | Usage                                              |
| ---     | ---  | ---           | ---                                                |
| 0x11000 | 0x4  | `00 50 07 00` | Address of failsafe FW update bank to use (u32 LE) |

## 0x12000-0x12FFF

Uninitialised on original factory firmware. After update first 2 bytes are initialised to `EF BE` (0xBEEF)

| Offset  | Size | Example | Usage            |
| ---     | ---  | ---     | ---              |
| 0x12000 | 0x2  | `EF BE` | Unknown (u16 LE) |

## 0x13000-0x14FFF

This section contains factory data defining the controller specific parameters such as serial, vendor & product ids, colours etc. Not all fields are initialised for all controller types.

| Offset  | Size | Example                                                                                                                   | Usage                              |
| ---     | ---  | ---                                                                                                                       | ---                                |
| 0x13000 | 0x2  | `01 00`                                                                                                                   | Unknown                            |
| 0x13002 | 0x10 | `48 45 4A 37 31 30 30 31 31 32 31 32 34 37 00 00`                                                                         | Serial number                      |
| 0x13012 | 0x2  | `7E 05`                                                                                                                   | Vendor ID                          |
| 0x13014 | 0x2  | `69 20`                                                                                                                   | Product ID                         |
| 0x13016 | 0x3  | `01 06 01`                                                                                                                | Unknown                            |
| 0x13019 | 0x3  | `23 23 23`                                                                                                                | Controller body colour             |
| 0x1301C | 0x3  | `A0 A0 A0`                                                                                                                | Controller buttons colour          |
| 0x1301F | 0x3  | `E6 E6 E6`                                                                                                                | Controller highlight colour        |
| 0x13022 | 0x3  | `32 32 32`                                                                                                                | Controller grip colour             |
| 0x13040 | 0x10 | `3B E0 D3 41 C6 60 6A BC 4D D7 A2 BB 71 1E DD 37`                                                                         | Unknown                            |
| 0x13060 | 0x4  | `4C 09 00 00`                                                                                                             | Unknown                            |
| 0x13080 | 0x28 | `01 AD D9 9A 55 56 65 A0 00 0A A0 00 0A E2 20 0E E2 20 0E 9A AD D9 9A AD D9 0A A5 50 0A A5 50 2F F6 62 2F F6 62 0A FF FF` | Unknown                            |
| 0x130A8 | 0x9  | `B3 67 83 2E 66 5E 3A 06 5F`                                                                                              | Primary analog stick calibration   |
| 0x130C0 | 0x28 | `01 AD D9 9A 55 56 65 A0 00 0A A0 00 0A E2 20 0E E2 20 0E 9A AD D9 9A AD D9 0A A5 50 0A A5 50 2F F6 62 2F F6 62 0A FF FF` | Unknown                            |
| 0x130E8 | 0x9  | `2C 08 84 D1 65 63 2A 26 62`                                                                                              | Secondary analog stick calibration |
| 0x13100 | 0x18 | `00 00 00 00 00 00 00 00 00 00 00 00 A6 F2 62 BD A8 00 08 3D 2F ED 20 41`                                                 | Unknown                            |
| 0x13140 | 0x9  | `00 D7 A3 BC 41 D7 A3 BC 41`                                                                                              | Unknown                            |
| 0x13E00 | 0x20 | `31 37 31 35 32 35 32 38 4C 44 31 37 34 33 31 30 31 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00`                         | Unknown serial                     |
| 0x13E20 | 0x4  | `01 02 02 02`                                                                                                             | Unknown                            |
| 0x13E30 | 0xA  | `03 02 04 01 05 02 06 02 07 02`                                                                                           | Unknown                            |
| 0x13E60 | 0x2  | `11 00`                                                                                                                   | Unknown                            |
| 0x13E80 | 0x9  | `00 23 E9 CE 00 41 30 4B 41`                                                                                              | Unknown                            |
| 0x13EFB | 0x4  | `01 00 15 0C`                                                                                                             | Unknown                            |

## 0x15000-0x74FFF

Contains similar header to section `0x0-0x10FFF`. Likely failsafe firmware update bank #1

## 0x75000-0xD4FFF

Uninitialised on original factory firmware. After firmware update contains similar header to section `0x0-0x10FFF`. Likely failsafe firmware update bank #2

## 0xD5000-0x174FFF

Unknown. Possibly another encrypted binary firmware blob

## 0x175000-0x1F9FFF

Uninitialised on original factory firmware. After firmware update contains an unencrypted firmware blob with the header magic `DSPH` and containing the string `MT3616A0 DSP`. Seems to be some kind of DSP firmware for the audio jack output.

## 0x1FA000-0x1FAFFF

Contains Bluetooth pairing info. Appears to be an item count, followed by an array of `count * 0x28` items. Each item contains the host address and LTK. By default there are 2 entries that share the same LTK. The first host address is the public console address seen during pairing. The second is always identical to the first, with the last byte decremented by one. This is likely some kind of private secondary interface.

| Offset   | Size | Example                                           | Usage           |
| ---      | ---  | ---                                               | ---             |
| 0x1FA000 | 0x1  | `02`                                              | Item count      |
| 0x1FA008 | 0x6  | `98 E2 55 2E 31 4B`                               | Host address #1 |
| 0x1FA01A | 0x10 | `D6 2C C6 34 D3 57 A0 48 E9 89 6F 98 79 6A 2D 71` | LTK #1          |
| 0x1FA030 | 0x6  | `98 E2 55 2E 31 4A`                               | Host address #2 |
| 0x1FA042 | 0x10 | `D6 2C C6 34 D3 57 A0 48 E9 89 6F 98 79 6A 2D 71` | LTK #2          |

## 0x1FB000-0x1FBFFF

Unknown. Data here seems unique to controller types, but doesn't change between controllers of the same type or firmware updates.

| Offset   | Size | Example                                                                               | Usage   |
| ---      | ---  | ---                                                                                   | ---     |
| 0x1FB000 | 0x8  | `02 00 02 00 01 00 11 03`                                                             | Unknown |
| 0x1FB00E | 0x2  | `00 00`                                                                               | Unknown |
| 0x1FB042 | 0x1C | `02 00 02 00 C8 00 78 00 99 0E D4 0D 4C 0D D5 0C 90 0C 66 0C FD 0B F7 0B AA 0B AA 0A` | Unknown |
| 0x1FB070 | 0x12 | `99 0E FF 0B FF FF 55 09 FF FF 55 0D 55 0C 3B 0C 00 00`                               | Unknown |

## 0x1FC000-0x1FCFFF

User calibration data. Uninitialised unless a user calibration has been performed.

| Offset   | Size | Example | Usage                              |
| ---      | ---  | ---     | ---                                |
| 0x1FC000 | 0x40 | ``      | Motion calibration                 |
| 0x1FC040 | 0xB  | ``      | Primary analog stick calibration   |
| 0x1FC060 | 0xB  | ``      | Secondary analog stick calibration |

## 0x1FD000-0x1FDFFF

Unitialised on original factory firmware except for the first 4 bytes at `0x1FD000-0x1FD003`, which are are `00 00 00 00`. These are cleared to `ff ff ff ff` after connecting to the console for the first time. After updating the controller firmware, bytes 0x1fd010-0x1fd013 are set to `00 00 00 00`

| Offset   | Size | Example       | Usage                                                                           |
| ---      | ---  | ---           | ---                                                                             |
| 0x1FD000 | 0x4  | `00 00 00 00` | Maybe shipment flag. Only set on factory firmware prior to pairing with console |
| 0x1FD010 | 0x4  | `00 00 00 00` | Unknown. Only set after updating controller firmware                            |

## 0x1FE000-0x1FEFFF

Unknown. Only present on some controllers.

| Offset   | Size  | Example                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Usage   |
| ---      | ---   | ---                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | ---     |
| 0x1FE000 | 0x100 | `5D 04 8B EA 3B 14 A2 00 92 95 9D F6 45 94 3D AD 15 1A BC 8E CE 50 B3 F3 2B 06 4C 27 27 51 86 9E CF A4 7B B6 DA 00 69 3F F2 52 FB 95 73 1A 0A DF F4 27 F9 FF E7 FA 7B 0B DE B2 40 05 32 44 56 C5 EB 54 D2 24 2F 40 C8 49 EA 07 5E 94 81 21 3D E7 3F 25 B2 75 AD 9C 99 04 B1 94 4E 8D 9C B7 8A D7 FE 50 1E A7 76 A4 D0 7A EA BB F3 9F 6F 4B 1F 57 A6 9A CE D0 19 27 FF 09 08 40 82 8B F0 54 CF BE 32 6D 62 7A 87 92 9D 75 FD 17 85 B2 73 C2 A3 B6 FE E5 ED 0F 2B 20 CA 03 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00` | Unknown |

## 0x1FF000-0x1FFFFF

Unknown. Only present on some controllers.

| Offset   | Size     | Example                                                                                                                                                                                                                                                                   | Usage   |
| ---      | ---      | ---                                                                                                                                                                                                                                                                       | ---     |
| 0x1FF000 | 0x58     | `FE CA 03 00 02 00 00 3F FE CA 0F 00 02 04 03 B9 FC A9 00 02 00 01 00 55 6E 6B 6E 59 FE CA 0F 00 02 04 03 33 84 B4 00 02 00 01 00 55 6E 6B 6E 47 FE CA 0F 00 02 04 03 59 CB AE 00 02 00 01 00 55 6E 6B 6E 2E FE CA 0F 00 02 04 03 58 08 A2 00 02 00 01 00 55 6E 6B 6E B8` | Unknown |
| 0x1FF400 | 0x490    | -                                                                                                                                                                                                                                                                         | Unknown |

Both offsets contain sequences of variable length blocks of data in the form

| Offset | Size     | Example    | Usage                               |
| ---    | ---      | ---        | ---                                 |
| 0x0    | 0x2      | `FE CA`    | Block start magic `0xCAFE` (u16 LE) |
| 0x2    | 0x2      | `03 00`    | Length of data to follow (u16 LE)   |
| 0x4    | 0x1      | `02`       | Unknown. Always `02`                |
| 0x5    | Variable | `00 00 3F` | Variable length data                |
