# HID Reports

Each controller has 2 input report formats and one output format available, as seen in the USB HID report [descriptors](descriptors.md#descriptors). The first report [`0x05`](#input-report-0x05) is common to all controllers. The other two are controller-specific. Report availability is outlined in the table below. By default, Nintendo uses input report ID #2 for both USB and Bluetooth connections. It is not known if input report ID #1 is currently used officially.

| Controller              | Input Report ID #1         | Input Report ID #2         | Output Report ID            |
| ---                     | ---                        | ---                        | ---                         |
| JoyCon 2 (L)            | [0x05](#input-report-0x05) | [0x07](#input-report-0x07) | [0x01](#output-report-0x01) |
| JoyCon 2 (R)            | [0x05](#input-report-0x05) | [0x08](#input-report-0x08) | [0x01](#output-report-0x01) |
| Pro Controller 2        | [0x05](#input-report-0x05) | [0x09](#input-report-0x09) | [0x02](#output-report-0x02) |
| NSO Gamecube Controller | [0x05](#input-report-0x05) | [0x0A](#input-report-0x0a) | [0x03](#output-report-0x03) |


USB input reports are activated by sending [command `0x03`](commands.md#command-0x03---initialisation), [subcommand `0x0D`](commands.md#subcommand-0x0d---initialise-usb). Input report IDs can be selected via [command `0x03`](commands.md#command-0x03---initialisation), [subcommand `0x0A`](commands.md#subcommand-0x0a---select-input-report).

Bluetooth reports omit the report ID, and are instead identified by a dedicated GATT characteristic and its associated UUID and/or handle. Input reports are activated by enabling notifications for the appropriate GATT characteristic (by writing the value `0x0001` to its associated Client Characteristic Configuration Descriptor). Output reports are sent by writing to the appropriate GATT characteristic. See the table below for the mapping between input/output report IDs and equivalent GATT characteristics.

| Report ID                   | Direction | Service UUID                           | Service Handle | Characteristic UUID                    | Characteristic Handle |
| ---                         | ---       | ---                                    | ---            | ---                                    |---                    |
| [0x01](#output-report-0x01) | Output    | `ab7de9be-89fe-49ad-828f-118f09df7fd0` | `0x0008`       | `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` | `0x0012`/`0x0016`*    |
| [0x02](#output-report-0x02) | Output    | `ab7de9be-89fe-49ad-828f-118f09df7fd0` | `0x0008`       | `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` | `0x0012`/`0x0016`*    |
| [0x03](#output-report-0x03) | Output    | `ab7de9be-89fe-49ad-828f-118f09df7fd0` | `0x0008`       | `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` | `0x0012`/`0x0016`*    |
| [0x05](#input-report-0x05)  | Input     | `ab7de9be-89fe-49ad-828f-118f09df7fd0` | `0x0008`       | `ab7de9be-89fe-49ad-828f-118f09df7fd2` | `0x000A`              |
| [0x07](#input-report-0x07)  | Input     | `ab7de9be-89fe-49ad-828f-118f09df7fd0` | `0x0008`       | `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` | `0x000E`              |
| [0x08](#input-report-0x08)  | Input     | `ab7de9be-89fe-49ad-828f-118f09df7fd0` | `0x0008`       | `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` | `0x000E`              |
| [0x09](#input-report-0x09)  | Input     | `ab7de9be-89fe-49ad-828f-118f09df7fd0` | `0x0008`       | `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` | `0x000E`              |
| [0x0A](#input-report-0x0a)  | Input     | `ab7de9be-89fe-49ad-828f-118f09df7fd0` | `0x0008`       | `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` | `0x000E`              |

\* Handle `0x0016` combines both HID output report data and controller [commands](commands.md#commands) into a single attribute write.

*Note: while the handle values appear to be fixed on the console, they are not guaranteed to be identical when connected to other hosts. Typically one would use the attribute UUID to uniquely identify a given attribute, but as can be seen from the UUIDs marked `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` in the table, many of the relevant characteristic UUIDs are non-static and appear to be randomised every time the controller is paired. It is not clear yet whether it's possible to resolve these back to a fixed value.*

---

## Input Report 0x05

Available on all controller types. Sent via GATT handle 0x000A (UUID=`ab7de9be-89fe-49ad-828f-118f09df7fd2`).

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

## Input Report 0x07

Only available on JoyCon 2 (L). Sent via GATT handle 0x000E (UUID randomised between devices/pairings)

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

---

## Input Report 0x08

Only available on JoyCon 2 (R). Sent via GATT handle 0x000E (UUID randomised between devices/pairings)

| Offset | Size | Value                              | Comment                                                      |
| ---    | ---  | ---                                | ---                                                          |
| 0x0    | 0x1  | Counter                            | -                                                            |
| 0x1    | 0x1  | Unknown                            | Power info?                                                  |
| 0x2    | 0x2  | [Buttons](#button-format-2)        | Bitfield                                                     |
| 0x4    | 0x1  | Unknown                            | Always 0x07?                                                 |
| 0x5    | 0x3  | Analog Stick                       | Packed 12-bit values (uncalibrated)                          |
| 0x8    | 0x1  | Unknown                            | -                                                            |
| 0x9    | 0x7  | [Mouse Data](#mouse-data-relative) | Activated via feature bit 4.                                 |
| 0xE    | 0x1  | Unknown                            | Always 0                                                     |
| 0xF    | 0x1  | Motion Data Length                 | Length of following motion data. Observed values {0, 30, 40} |
| 0x10   | 0x28 | Motion Data                        | Activated via feature bit 2. Unknown packed format           |
| 0x38   | 0x7  | Reserved                           | Unused                                                       |

#### Button Format

| Byte | 0x80   | 0x40 | 0x20 | 0x10 | 0x08 | 0x04 | 0x02 | 0x01 |
| ---  | ---    | ---  | ---  | ---  | ---  | ---  | ---  |  --- |
| 0    | Stick  | Plus | ZR   | R    | X    | Y    | A    | B    |
| 1    | SL     | SR   | -    | C    | -    | -    | -    | Home |

#### Mouse Data (relative)

| Offset | Size | Value   | Comment            |
| ---    | ---  | ---     | ---                |
| 0x0    | 0x2  | Delta X | -                  |
| 0x2    | 0x2  | Delta Y | -                  |
| 0x4    | 0x1  | Unknown | Lift-off distance? |

---

## Input Report 0x09

Only available on Pro Controller 2. Sent via GATT handle 0x000E (UUID randomised between devices/pairings)

| Offset | Size | Value                       | Comment                                                      |
| ---    | ---  | ---                         | ---                                                          |
| 0x0    | 0x1  | Counter                     | -                                                            |
| 0x1    | 0x1  | Power Info                  | Similar to Switch 1 format                                   |
| 0x2    | 0x3  | [Buttons](#button-format-3) | Bitfield                                                     |
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

---

## Input Report 0x0A

Only available on NSO Gamecube controllers. Sent via GATT handle 0x000E (UUID randomised between devices/pairings)

| Offset | Size | Value                       | Comment                                                      |
| ---    | ---  | ---                         | ---                                                          |
| 0x0    | 0x1  | Counter                     | -                                                            |
| 0x1    | 0x1  | Power Info                  | Similar to Switch 1 format                                   |
| 0x2    | 0x3  | [Buttons](#button-format-4) | Bitfield                                                     |
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

---

## Output Report 0x01

Only available on JoyCon 2.

---

## Output Report 0x02

Only available on Pro Controller 2.

---

## Output Report 0x03

Only available on NSO Gamecube controllers.
