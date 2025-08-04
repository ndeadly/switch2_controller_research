# HID Input Reports

There are two HID input report formats, with IDs `0x05` and `0x09`. Both reports are the same between USB and Bluetooth interfaces, except Bluetooth reports omit the first byte containing the report ID since they are uniquely identified by their associated GATT characteristic and its UUID or handle. The offsets listed here don't account for the report ID, so must be incremented by one if you are working with packets transmitted over USB.

USB reports are activated by sending command `0x03` to the command endpoint. Official software initially sends `03 91 00 0D 00 08 00 00 01 00 FF FF FF FF FF FF`, where the `0xFF` values are the host Bluetooth address in little-endian byte order. However, this seem to be related to pairing and `03 91 00 0A 00 04 00 00 09 00 00 00` can be used instead to simultaneously activate input reports and set the report ID via byte 8.

Bluetooth reports are activated by enabling notifications for the appropriate GATT characteristic, by writing the value `0x0001` to its associated Client Characteristic Configuration Descriptor (UUID=`0x2902`). Enabling notifications for one characteristic will disable them for the other, if enabled.

By default, Nintendo uses [report 0x09](#input-report-0x09) on both USB and Bluetooth interfaces. It is not known if [report 0x05](#input-report-0x05) is currently used officially.

---

## Input Report 0x05

Sent via GATT handle 0x000A (UUID=`ab7de9be-89fe-49ad-828f-118f09df7fd2`). Common format across all controller types.

| Offset | Size | Value                                   | Comment                                                                                             |
| ---    | ---  | ---                                     | ---                                                                                                 |
| 0x0    | 0x4  | Counter                                 | -                                                                                                   |
| 0x4    | 0x4  | [Buttons](#button-format)               | Bitfield                                                                                            |
| 0x8    | 0x2  | Unknown                                 | -                                                                                                   |
| 0xA    | 0x3  | Left Analog Stick                       | Packed 12-bit values (uncalibrated)                                                                 |
| 0xD    | 0x3  | Right Analog Stick                      | Packed 12-bit values (uncalibrated)                                                                 |
| 0x10   | 0x8  | [Mouse Data](#mouse-data-absolute)      | Activated via feature bit 4. Only present on Joycon controllers                                     |
| 0x18   | 0x1  | Unknown                                 | Always 0                                                                                            |
| 0x19   | 0x6  | [Magnetometer Data](#magnetometer-data) | Activated via feature bit 7                                                                         |
| 0x1F   | 0x2  | Battery Voltage                         | Battery voltage in mV                                                                               |
| 0x21   | 0x1  | Charging State/Rate                     | Changes when USB power source connected. Seems to rise and settle on 0x34. 0x20 when fully charged. |
| 0x22   | 0x2  | Battery Current?                        | Activated via feature bit 5                                                                         |
| 0x24   | 0x5  | Unknown                                 | Always 0                                                                                            |
| 0x29   | 0x1  | Unknown                                 | Always 0x01                                                                                         |
| 0x2A   | 0x12 | [Motion Data](#motion-data)             | Activated via feature bit 2                                                                         |
| 0x3C   | 0x1  | Left Analog Trigger                     | Only present on Gamecube NSO controller (uncalibrated)                                              |
| 0x3D   | 0x1  | Right Analog Trigger                    | Only present on Gamecube NSO controller (uncalibrated)                                              |
| 0x3E   | 0x1  | Reserved                                | Unused                                                                                              |

#### Button Format

| Byte | 0x80 | 0x40 | 0x20     | 0x10     | 0x08       | 0x04        | 0x02    | 0x01  |
| ---  | ---  | ---  | ---      | ---      | ---        | ---         | ---     |  ---  |
| 0    | ZR   | R    | SL Right | SR Right | A          | B           | X       | Y     |
| 1    | -    | C    | Capture  | Home     | Left Stick | Right Stick | Plus    | Minus |
| 2    | ZL   | L    | SL Left  | SR Left  | Left       | Right       | Up      | Down  |
| 3    | -    | -    | -        | Headset  | -          | -           | GL      | GR    |

#### Mouse Data (absolute)

| Offset | Size | Value      | Comment             |
| ---    | ---  | ---        | ---                 |
| 0x0    | 0x2  | Position X | Absolute x position |
| 0x2    | 0x2  | Position Y | Absolute y position |
| 0x4    | 0x2  | Unknown    | Surface quality?    |
| 0x6    | 0x2  | Unknown    | Lift-off distance?  |

#### Magnetometer Data

| Offset | Size | Value | Comment |
| ---    | ---  | ---   | ---     |
| 0x0    | 0x2  | X     | -       |
| 0x2    | 0x2  | Y     | -       |
| 0x4    | 0x2  | Z     | -       |

#### Motion Data

| Offset | Size | Value       | Comment |
| ---    | ---  | ---         | ---     |
| 0x0    | 0x4  | Timestamp   | -       |
| 0x4    | 0x2  | Temperature | -       |
| 0x6    | 0x2  | Accel X     | -       |
| 0x8    | 0x2  | Accel Y     | -       |
| 0xA    | 0x2  | Accel Z     | -       |
| 0xC    | 0x2  | Gyro X      | -       |
| 0xE    | 0x2  | Gyro Y      | -       |
| 0x10   | 0x2  | Gyro Z      | -       |

---

## Input Report #0x09

Sent via GATT handle 0x000E (UUID randomised between devices/pairings). Always length 0x3F but report format changes depending upon controller type. Pro controller and Gamecube reports are almost identical apart from analog triggers and headset fields but have been documented separately along with controller-specific button labelling to avoid confusion.


### Joycon Report

| Offset | Size | Value                              | Comment                                                      |
| ---    | ---  | ---                                | ---                                                          |
| 0x0    | 0x1  | Counter                            | -                                                            |
| 0x1    | 0x1  | Unknown                            | Power info?                                                  |
| 0x2    | 0x2  | [Buttons](#button-format-1)        | Bitfield                                                     |
| 0x4    | 0x1  | Unknown                            | Always 0x07?                                                 |
| 0x5    | 0x3  | Analog Stick                       | Packed 12-bit values (uncalibrated)                          |
| 0x8    | 0x1  | Unknown                            | -                                                            |
| 0x9    | 0x7  | [Mouse Data](#mouse-data-relative) | Activated via feature bit 4.                                 |
| 0xE    | 0x1  | Unknown                            | Always 0                                                     |
| 0xF    | 0x1  | Motion Data Length                 | Length of following motion data. Observed values {0, 30, 40} |
| 0x10   | 0x28 | Motion Data                        | Activated via feature bit 2. Unknown packed format           |
| 0x38   | 0x7  | Reserved                           | Unused                                                       |

#### Button Format

##### JoyCon (R)

| Byte | 0x80   | 0x40 | 0x20 | 0x10 | 0x08 | 0x04 | 0x02 | 0x01 |
| ---  | ---    | ---  | ---  | ---  | ---  | ---  | ---  |  --- |
| 0    | Stick  | Plus | ZR   | R    | X    | Y    | A    | B    |
| 1    | SL     | SR   | -    | C    | -    | -    | -    | Home |

##### JoyCon (L)

| Byte | 0x80  | 0x40  | 0x20 | 0x10 | 0x08 | 0x04 | 0x02  | 0x01    |
| ---  | ---   | ---   | ---  | ---  | ---  | ---  | ---   |  ---    |
| 0    | Stick | Minus | ZL   | L    | Up   | Left | Right | Down    |
| 1    | SL    | SR    | -    | -    | -    | -    | -     | Capture |

#### Mouse Data (relative)

| Offset | Size | Value   | Comment            |
| ---    | ---  | ---     | ---                |
| 0x0    | 0x2  | Delta X | -                  |
| 0x2    | 0x2  | Delta Y | -                  |
| 0x4    | 0x1  | Unknown | Lift-off distance? |


### Pro Controller Report

| Offset | Size | Value                       | Comment                                                      |
| ---    | ---  | ---                         | ---                                                          |
| 0x0    | 0x1  | Counter                     | -                                                            |
| 0x1    | 0x1  | Power Info                  | Similar to Switch 1 format                                   |
| 0x2    | 0x3  | [Buttons](#button-format-2) | Bitfield                                                     |
| 0x5    | 0x3  | Left Analog Stick           | Packed 12-bit values (uncalibrated)                          |
| 0x8    | 0x3  | Right Analog Stick          | Packed 12-bit values (uncalibrated)                          |
| 0xB    | 0x1  | Unknown                     | Always 0x38                                                  |
| 0xC    | 0x1  | Unknown                     | Unused?                                                      |
| 0xD    | 0x1  | Headset/Flags               | Only present on Pro Controller. 0x1 when headset inserted    |
| 0xE    | 0x1  | Motion Data Length          | Length of following motion data. Observed values {0, 30, 40} |
| 0xF    | 0x28 | Motion Data                 | Activated via feature bit 2. Unknown packed format           |
| 0x37   | 0x8  | Reserved                    | Unused                                                       |

#### Button Format

| Byte | 0x80        | 0x40  | 0x20 | 0x10 | 0x08 | 0x04 | 0x02    | 0x01 |
| ---  | ---         | ---   | ---  | ---  | ---  | ---  | ---     |  --- |
| 0    | Right Stick | Plus  | ZR   | R    | X    | Y    | A       | B    |
| 1    | Left Stick  | Minus | ZL   | L    | Up   | Left | Right   | Down |
| 2    | -           | -     | -    | C    | GL   | GR   | Capture | Home |


### NSO Gamecube Controller Report

| Offset | Size | Value                       | Comment                                                      |
| ---    | ---  | ---                         | ---                                                          |
| 0x0    | 0x1  | Counter                     | -                                                            |
| 0x1    | 0x1  | Power Info                  | Similar to Switch 1 format                                   |
| 0x2    | 0x3  | [Buttons](#button-format-3) | Bitfield                                                     |
| 0x5    | 0x3  | Left Analog Stick           | Packed 12-bit values (uncalibrated)                          |
| 0x8    | 0x3  | Right Analog Stick          | Packed 12-bit values (uncalibrated)                          |
| 0xB    | 0x1  | Unknown                     | Always 0x38                                                  |
| 0xC    | 0x1  | Left Analog Trigger         | Gamecube analog trigger (uncalibrated)                       |
| 0xD    | 0x1  | Right Analog Trigger        | Gamecube analog trigger (uncalibrated)                       |
| 0xE    | 0x1  | Motion Data Length          | Length of following motion data. Observed values {0, 30, 40} |
| 0xF    | 0x28 | Motion Data                 | Activated via feature bit 2. Unknown packed format           |
| 0x37   | 0x8  | Reserved                    | Unused                                                       |

#### Button Format

| Byte | 0x80        | 0x40  | 0x20 | 0x10 | 0x08 | 0x04 | 0x02    | 0x01 |
| ---  | ---         | ---   | ---  | ---  | ---  | ---  | ---     |  --- |
| 0    | Right Stick | Plus  | R    | Z    | X    | Y    | A       | B    |
| 1    | Left Stick  | Minus | L    | ZL   | Up   | Left | Right   | Down |
| 2    | -           | -     | -    | C    | -    | -    | Capture | Home |
