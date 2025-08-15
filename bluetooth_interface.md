# Bluetooth Interface

Unlike Switch 1 controllers that used Bluetooth BD/EDR, the new Switch 2 controllers now use Bluetooth LE for wireless connections. However, rather than following standards defined in the Bluetooth specification, Nintendo have opted to implement their own proprietary versions of everything, as is tradition.

These choices present the following hurdles for supporting Switch 2 controllers on other platforms, or building a device or piece of software that mimics a controller:

- There are no standard identifying fields present in the [advertisements](#bluetooth-le-advertisements) broadcast by the controller that a generic driver could use to identify the type of device.
- The controllers implement their own (Pseudo)-Out-Of-Band [pairing procedure](#pairing) over the HID command interface instead of using the standard [Security Manager Protocol (SMP)](https://www.bluetooth.com/wp-content/uploads/Files/Specification/HTML/Core-54/out/en/host/security-manager-specification.html#UUID-357f5fc6-5b55-6d33-e459-f42c929137f8), which is not supported at all. Attempting to pair controllers using SMP (as many platforms do automatically) will cause the controller to terminate the connection.
- Rather than exposing the standard [HID over GATT Profile](https://www.bluetooth.com/specifications/specs/html/?src=HOGP_v1.0/out/en/index-en.html), the controllers implement their own HID-like interface via a number of proprietary [GATT attributes](#gatt-attributes). Because of this they won't work with any generic HID drivers.
- The console uses a vendor-specific HCI command to set the LE Connection Parameters for the connection. Because of this, the controller never explicitly issues the standard Connection Parameter Update Request that many Bluetooth stacks require to override the default parameters. On Windows 10, for example, the default connection interval hardcoded in `bthport.sys` is 60ms and there is no documented way to explicitly set the connection parameters, so the reporting rate is limited to ~16.66Hz.
- The connection interval used by the console (5ms) while allowing for lower input latencies, is below the minimum 7.5ms defined in the Bluetooth Specification and may be problematic with some Bluetooth hardware.
- The final attribute in the [GATT attribute list](#gatt-attributes), the `Generic Attribute` service, doesn't have any child attributes beneath it. This may be cause issues if an implementation assumes all services must have at least one child attribute.

---

## Bluetooth LE Advertisements

 In addition to being used during device discovery, Bluetooth LE advertisements are also used during reconnection and waking the console. The advertising data is similar for each function and consists of only two entries, Flags and Manufacturer Data. The different types of advertisements are outlined below.

The Switch 2 console uses Bluetooth LE advertisement filtering to limit traffic to Nintendo devices and exclude unwanted devices at the SoC level. For advertisements to be accepted by the filter, they must include a manufacturer specific data entry (AD Type=`0xFF`) 

#### Controller -> Console

These are the known advertisements and their function used by Switch 2 controllers. Controller type can be determined during discovery by checking the vendor and product ID fields of the [manufacturer data](#manufacturer-data-format).

| Advertisement Type | PDU Type  | Flags (AD Type=`0x01`)                           | Manufacturer Data (AD Type=`0xFF`)                                              |
| ---                | ---       | ---                                              | ---                                                                             |
| Standard           | `ADV_IND` | `06` (`GeneralDiscovery` \| `BrEdrNotSupported`) | `53 05 01 00 03 7e 05 69 20 00 01 00 00 00 00 00 00 00 0f 00 00 00 00 00 00 00` |
| Reconnection       | `ADV_IND` | `06` (`GeneralDiscovery` \| `BrEdrNotSupported`) | `53 05 01 00 03 7e 05 69 20 00 01 00 5f 11 85 eb f1 48 0f 00 00 00 00 00 00 00` |
| Wake Console       | `ADV_IND` | `06` (`GeneralDiscovery` \| `BrEdrNotSupported`) | `53 05 01 00 03 7e 05 69 20 00 01 81 5f 11 85 eb f1 48 0f 00 00 00 00 00 00 00` |

##### Manufacturer Data Format:

| Offset | Size | Value           | Comment                                                                             |
| ---    | ---  | ---             | ---                                                                                 |
| 0x0    | 0x2  | Manufacturer ID | Always `0x0553` (Nintendo)                                                          |
| 0x2    | 0x1  | Unknown         | Always `0x01`                                                                       |
| 0x3    | 0x1  | Unknown         | Always `0x00`                                                                       |
| 0x4    | 0x1  | Unknown         | Always `0x03`                                                                       |
| 0x5    | 0x2  | Vendor ID       | Always `0x057E` (Nintendo)                                                          |
| 0x7    | 0x2  | Product ID      | Switch 2 product IDs begin from `0x2060`                                            |
| 0x9    | 0x1  | Unknown         | Always `0x00`                                                                       |
| 0xA    | 0x1  | Unknown         | Always `0x01`                                                                       |
| 0xB    | 0x1  | Unknown         | `0x81` for wake advertisement, `0x00` otherwise                                     |
| 0xC    | 0x6  | Address         | Host Bluetooth address (reverse byte-order). All `0x00` for standard advertisements |
| 0x12   | 0x1  | Unknown         | Always `0x0F`                                                                       |
| 0x13   | 0x7  | Reserved        | All `0x00`                                                                          |

#### Console -> Controller

The console can also send its own Bluetooth LE advertisements to the controller, for example when using the "Search for controllers" feature to find and wake a controller. This advertisement is detailed below.

| Advertisement Type | PDU Type          | Flags (AD Type=`0x01`)                                      | Manufacturer Data (AD Type=`0xFF`)          |
| ---                | ---               | ---                                                         | ---                                         |
| Wake Controller    | `ADV_NONCONN_IND` | `18` (`DualModeControllerSupport` \| `DualModeHostSupport`) | `53 05 03 00 01 00 80 88 16 c2 55 e2 98 01` |

##### Manufacturer Data Format:

| Offset | Size | Value           | Comment                                           |
| ---    | ---  | ---             | ---                                               |
| 0x0    | 0x2  | Manufacturer ID | Always `0x0553` (Nintendo)                        |
| 0x2    | 0x1  | Unknown         | Always `0x03`                                     |
| 0x3    | 0x1  | Unknown         | Always `0x00`                                     |
| 0x4    | 0x1  | Unknown         | Always `0x01`                                     |
| 0x5    | 0x1  | Unknown         | Always `0x00`                                     |
| 0x6    | 0x1  | Unknown         | Always `0x80`                                     |
| 0x7    | 0x6  | Address         | Controller Bluetooth Address (reverse byte-order) |
| 0x12   | 0x1  | Unknown         | Always `0x01`                                     |

---

## Pairing

Switch 2 controllers support Bluetooth pairing, however Nintendo implements a psuedo Out-Of-Band exchange of key material via their custom HID-like command interface rather than the standard SMP protocol used by most LE devices. Any attempts to initiate pairing via SMP as many stack implementations do by default will cause the controller to disconnect from the host.

Pairing itself is optional for communicating with the controller and receiving notifications etc. via PC and other devices, but the Switch 2 console requires successful pairing in order to complete initialisation, and valid pairing entries are required in order for the controller to automatically reconnect to the console or wake it from sleep.

The pairing procedure can be performed via both Bluetooth and USB connections and is outlined below
1) The console sends a list of 2 host bluetooth addresses to the controller via pairing subcommand [`0x01`](commands.md#subcommand-0x01---exchange-addresses)
2) The controller responds with its own bluetooth address
3) The console generates a 16-byte public key (`A1`) and sends it to the controller via pairing subcommand [`0x04`](commands.md#subcommand-0x04---exchange-keys)
4) The controller responds with its own 16-byte public key (`B1`, appears to be fixed as `5CF6EE792CDF05E1BA2B6325C41A5F10`)
5) Both console and controller can now compute the common `LTK` as `A1 ^ B1`
6) The console generates a 16-byte challenge (`A2`) and sends it to the controller via pairing subcommand [`0x02`](commands.md#subcommand-0x02---confirm-ltk)
7) The controller computes the confirmation response (`B2`) by encrypting `A2` with the `LTK` using AES128 in ECB mode. Note that both `A2` and `LTK` are _byte reversed_ for this computation
8) The console verifies the confirmation response (decrypting `B2` with the `LTK` should yield the original challenge `A2`) and finalises the pairing via pairing subcommand [`0x03`](commands.md#subcommand-0x03---finalise-pairing), triggering the controller to commit the host addresses and `LTK` to internal storage at address [`0x1FA000`](memory_layout.md#0x1FA000-0x1FAFFF)

Below is some simple python code illustrating how the `LTK` and confirm response (`B2`) are generated from the exchanged data

```python
from Crypto.Cipher import AES

A1 = bytes.fromhex("3503e92982877124bea80c664615834b")  # 16-byte public key from host
B1 = bytes.fromhex("5cf6ee792cdf05e1ba2b6325c41a5f10")  # 16-byte public key from controller
A2 = bytes.fromhex("6fc6df8ad8fedf15bb8c15e91f320544")  # 16-byte challenge from the host

LTK = bytes(a ^ b for a, b in zip(A1, B1))  # Both devices compute the common LTK by XORing their own public key with the one shared by the other
B2 = AES.new(LTK[::-1], AES.MODE_ECB).encrypt(A2[::-1])  # The controller encrypts the challenge (A2) with the LTK to prove to the host it possesses the correct LTK

print("LTK:", LTK.hex())
print("B2: ", B2.hex())
```

---

## GATT Attributes

Once a connection is established, all controller types expose the following GATT attributes. Attributes with handles `0x000e`, `0x0012`, `0x0016` and `0x001e` (marked `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`) appear to be non-static, and have a new UUID generated every time the controller is paired to a new host.

| UUID                                   | Type            | Handle(s)     | Properties      | Usage                                                 | Controller |
| ---                                    | ---             | ---           | ---             | ---                                                   | ---        |
| `00c5af5d-1964-4e30-8f51-1956f96bd280` | Primary Service | 0x0001-0x0007 | -               | Unknown                                               | All        |
| `00c5af5d-1964-4e30-8f51-1956f96bd281` | Characteristic  | 0x0003        | READ            | Unknown                                               | All        |
| `00c5af5d-1964-4e30-8f51-1956f96bd282` | Characteristic  | 0x0005        | WRITE           | Unknown                                               | All        |
| `00c5af5d-1964-4e30-8f51-1956f96bd283` | Characteristic  | 0x0007        | READ            | Unknown                                               | All        |
| `ab7de9be-89fe-49ad-828f-118f09df7fd0` | Primary Service | 0x0008-0x002a | -               | Unknown                                               | All        |
| `ab7de9be-89fe-49ad-828f-118f09df7fd2` | Characteristic  | 0x000a        | READ \| NOTIFY  | [Input Report 0x05](hid_reports.md#input-report-0x05) | All        |
| `2902`                                 | Descriptor      | 0x000b        | -               | Client Characteristic Configuration                   | All        |
| `679d5510-5a24-4dee-9557-95df80486ecb` | Descriptor      | 0x000c        | -               | Set Report Rate?                                      | All        |
| `cc1bbbb5-7354-4d32-a716-a81cb241a32a` | Characteristic  | 0x000e        | READ \| NOTIFY  | [Input Report 0x07](hid_reports.md#input-report-0x07) | JC (L)     |
| `d5a9e01e-2ffc-4cca-b20c-8b67142bf442` | Characteristic  | 0x000e        | READ \| NOTIFY  | [Input Report 0x08](hid_reports.md#input-report-0x08) | JC (R)     |
| `7492866c-ec3e-4619-8258-32755ffcc0f8` | Characteristic  | 0x000e        | READ \| NOTIFY  | [Input Report 0x09](hid_reports.md#input-report-0x09) | Pro        |
| `8261cba1-9435-420c-84d6-f0c75a2c8e4d` | Characteristic  | 0x000e        | READ \| NOTIFY  | [Input Report 0x0A](hid_reports.md#input-report-0x0a) | GC         |
| `2902`                                 | Descriptor      | 0x000f        | -               | Client Characteristic Configuration                   | All        |
| `679d5510-5a24-4dee-9557-95df80486ecb` | Descriptor      | 0x0010        | -               | Set Report Rate?                                      | All        |
| `289326cb-a471-485d-a8f4-240c14f18241` | Characteristic  | 0x0012        | WRITENORESPONSE | Output Report (Vibration)                             | JC (L)     |
| `fa19b0fb-cd1f-46a7-84a1-bbb09e00c149` | Characteristic  | 0x0012        | WRITENORESPONSE | Output Report (Vibration)                             | JC (R)     |
| `cc483f51-9258-427d-a939-630c31f72b05` | Characteristic  | 0x0012        | WRITENORESPONSE | Output Report (Vibration)                             | Pro        |
| `3f8fb670-ab25-45bf-b540-38c72834d064` | Characteristic  | 0x0012        | WRITENORESPONSE | Output Report (Vibration)                             | GC         |
| `649d4ac9-8eb7-4e6c-af44-1ea54fe5f005` | Characteristic  | 0x0014        | WRITENORESPONSE | Output Report (Command)                               | All        |
| `ce49a830-dced-48ae-931e-c8cf88aadbea` | Characteristic  | 0x0016        | WRITENORESPONSE | Output Report (Vibration + Command)                   | JC (L)     |
| `65a724b3-f1e7-4a61-8078-a342376b27ff` | Characteristic  | 0x0016        | WRITENORESPONSE | Output Report (Vibration + Command)                   | JC (R)     |
| `3dacbc7e-6955-40b5-8eaf-6f9809e8b379` | Characteristic  | 0x0016        | WRITENORESPONSE | Output Report (Vibration + Command)                   | Pro        |
| `af95885e-44b3-4a24-9cf0-483cc129469a` | Characteristic  | 0x0016        | WRITENORESPONSE | Output Report (Vibration + Command)                   | GC         |
| `4147423d-fdae-4df7-a4f7-d23e5df59f8d` | Characteristic  | 0x0018        | WRITENORESPONSE | Output Report (Firmware Update)                       | All        |
| `c765a961-d9d8-4d36-a20a-5315b111836a` | Characteristic  | 0x001a        | NOTIFY          | Input Report (Command Response #1)                    | All        |
| `2902`                                 | Descriptor      | 0x001b        | -               | Client Characteristic Configuration                   | All        |
| `b746df8c-f358-495b-9cd2-e3bbeda4f979` | Descriptor      | 0x001c        | -               | Unknown                                               | All        |
| `63a3810f-aec7-474b-9010-3d52403cb996` | Characteristic  | 0x001e        | NOTIFY          | Input Report (Command Response #2)                    | JC (L)     |
| `640ca58e-0e88-410c-a7f3-426faf2b690b` | Characteristic  | 0x001e        | NOTIFY          | Input Report (Command Response #2)                    | JC (R)     |
| `506d9f7d-4278-4e95-a549-326ba77657e0` | Characteristic  | 0x001e        | NOTIFY          | Input Report (Command Response #2)                    | Pro        |
| `46f6ad29-cdaf-4569-a2fe-339020b94604` | Characteristic  | 0x001e        | NOTIFY          | Input Report (Command Response #2)                    | GC         |
| `2902`                                 | Descriptor      | 0x001f        | -               | Client Characteristic Configuration                   | All        |
| `b746df8c-f358-495b-9cd2-e3bbeda4f979` | Descriptor      | 0x0020        | -               | Unknown                                               | All        |
| `d3bd69d2-841c-4241-ab15-f86f406d2a80` | Characteristic  | 0x0022        | NOTIFY          | Input Report (Unknown)                                | All        |
| `2902`                                 | Descriptor      | 0x0023        | -               | Client Characteristic Configuration                   | All        |
| `b746df8c-f358-495b-9cd2-e3bbeda4f979` | Descriptor      | 0x0024        | -               | Unknown                                               | All        |
| `ab7de9be-89fe-49ad-828f-118f09df7fde` | Characteristic  | 0x0026        | READ \| NOTIFY  | Input Report (Unknown)                                | All        |
| `2902`                                 | Descriptor      | 0x0027        | -               | Client Characteristic Configuration                   | All        |
| `679d5510-5a24-4dee-9557-95df80486ecb` | Descriptor      | 0x0028        | -               | Unknown                                               | All        |
| `ab7de9be-89fe-49ad-828f-118f09df7fdf` | Characteristic  | 0x002a        | WRITENORESPONSE | Output Report (Unknown)                               | All        |
| `cc483f51-9258-427d-a939-630c31f72b06` | Characteristic  | 0x002c        | WRITENORESPONSE | Output Report (Headset Audio)                         | Pro        |
| `7492866c-ec3e-4619-8258-32755ffcc0f9` | Characteristic  | 0x002e        | READ \| NOTIFY  | Input Report (Headset Audio)                          | Pro        |
| `2902`                                 | Descriptor      | 0x002f        | -               | Client Characteristic Configuration                   | Pro        |
| `679d5510-5a24-4dee-9557-95df80486ecb` | Descriptor      | 0x0030        | -               | Set Report Rate?                                      | Pro        |
| `3dacbc7e-6955-40b5-8eaf-6f9809e8b380` | Characteristic  | 0x0032        | WRITENORESPONSE | Output Report (Unknown)                               | Pro        |

#### Standard Attributes

The controllers also expose the following standard attributes from the spec. Curiously, the Generic Attribute service has no attributes underneath it. Pro Controllers that have been updated from the original factory firmware to support headset audio have some additional attributes added, so the handle values below are offset by +8, starting from `0x0033`.

| UUID                                   | Type            | Handle(s)     | Properties      | Usage                                                 | Controller |
| ---                                    | ---             | ---           | ---             | ---                                                   | ---        |
| `1800`                                 | Primary Service | 0x002b-0x002f | -               | Generic Access                                        | All        |
| `2a00`                                 | Characteristic  | 0x002d        | READ            | Device Name                                           | All        |
| `2a01`                                 | Characteristic  | 0x002f        | READ            | Appearance                                            | All        |
| `1801`                                 | Primary Service | 0x0030-0x0030 | -               | Generic Attribute                                     | All        |

---

## Initialisation Sequence

Official software initalises the controllers via Bluetooth with the following sequences of GATT attribute write requests.

Notes:
- All commands written to handle `0x0016` receive a corresponding response on handle `0x001E`
- Commands written to `0x0016` are prefixed with with a number of `00` bytes, depending on the controller type
- Command responses on handle `0x001E` are also prefixed with `00` bytes
- `0x15` commands are only used during initial pairing and are omitted on reconnection.

#### Pro Controller 2

| Handle   | Example Data                                                                          | Comment                                                       |
| ---      | ---                                                                                   | ---                                                           |
| `0x0005` | `01 00`                                                                               |                                                               |
| `0x001F` | `01 00`                                                                               | Enable notifications for command responses on handle `0x001E` |
| `0x0016` | `07 91 01 01 00 00 00 00`                                                             |                                                               |
| `0x0016` | `02 91 01 04 00 08 00 00 40 7e 00 00 00 30 01 00`                                     | Read 0x40 bytes from `0x13000`                                |
| `0x0016` | `16 91 01 01 00 00 00 00`                                                             |                                                               |
| `0x0016` | `15 91 01 01 00 0e 00 00 00 02 81 eb 3a eb f1 48 80 eb 3a eb f1 48`                   | Exchange Bluetooth addresses                                  |
| `0x0016` | `15 91 01 04 00 11 00 00 00 35 03 e9 29 82 87 71 24 be a8 0c 66 46 15 83 4b`          | Exchange Bluetooth LTK components                             |
| `0x0016` | `15 01 01 02 10 78 00 00 01 13 4c 97 f5 11 b9 b6 dd 4d 86 fd 40 f5 36 e9 ed`          | Exchange Bluetooth LTK confirm challenge/response             |
| `0x0016` | `15 91 01 03 00 01 00 00 00`                                                          | Finalise Bluetooth pairing                                    |
| `0x0016` | `0a 91 01 02 00 04 00 00 03 00 00 00`                                                 | Play connection vibration sample                              |
| `0x0016` | `09 91 01 07 00 08 00 00 01 00 00 00 00 00 00 00`                                     | Set player LEDs                                               |
| `0x0016` | `0c 91 01 02 00 04 00 00 2f 00 00 00`                                                 | Initialise feature flags (`0x2F`)                             |
| `0x0016` | `02 91 01 04 00 08 00 00 40 7e 00 00 80 30 01 00`                                     | Read 0x40 bytes from `0x13080`                                |
| `0x0016` | `02 91 01 04 00 08 00 00 40 7e 00 00 c0 30 01 00`                                     | Read 0x40 bytes from `0x130C0`                                |
| `0x0016` | `02 91 01 04 00 08 00 00 40 7e 00 00 40 c0 1f 00`                                     | Read 0x40 bytes from `0x1fC040`                               |
| `0x0016` | `02 91 01 04 00 08 00 00 10 7e 00 00 40 30 01 00`                                     | Read 0x10 bytes from `0x13040`                                |
| `0x0016` | `02 91 01 04 00 08 00 00 18 7e 00 00 00 31 01 00`                                     | Read 0x18 bytes from `0x13100`                                |
| `0x0016` | `11 91 01 03 00 00 00 00`                                                             |                                                               |
| `0x0016` | `02 91 01 04 00 08 00 00 20 7e 00 00 60 30 01 00`                                     | Read 0x20 bytes from `0x13060`                                |
| `0x0016` | `0a 91 01 08 00 14 00 00 01 ff ff ff ff ff ff ff ff 35 00 46 00 00 00 00 00 00 00 00` | Send vibration data                                           |
| `0x0016` | `0c 91 01 04 00 04 00 00 2f 00 00 00`                                                 | Confirm feature flags (`0x2F`)                                |
| `0x0010` | `85 00`                                                                               |                                                               |
| `0x000F` | `01 00`                                                                               | Enable notifications for HID input reports on handle `0x000E` |

#### JoyCon 2

| Handle   | Example Data                                                                                | Comment                                                       |
| ---      | ---                                                                                         | ---                                                           |
| `0x0005` | `01 00`                                                                                     |                                                               |
| `0x001B` | `01 00`                                                                                     | Enable notifications for command responses on handle `0x001A` |
| `0x001F` | `01 00`                                                                                     | Enable notifications for command responses on handle `0x001E` |
| `0x0023` | `01 00`                                                                                     | Enable notifications for command responses on handle `0x0022` |
| `0x0016` | `07 91 01 01 00 00 00 00`                                                                   |                                                               |
| `0x0016` | `02 91 01 04 00 08 00 00 40 7e 00 00 00 30 01 00`                                           | Read `0x40` bytes from `0x13000`                              |
| `0x0016` | `10 91 01 01 00 00 00 00`                                                                   |                                                               |
| `0x0016` | `16 91 01 01 00 00 00 00`                                                                   |                                                               |
| `0x0016` | `15 91 01 01 00 0e 00 00 00 02 5f 11 85 eb f1 48 5e 11 85 eb f1 48`                         | Exchange Bluetooth addresses                                  |
| `0x0016` | `15 91 01 04 00 11 00 00 00 08 06 5a 60 e9 02 e4 e1 02 02 9e 3f a3 9a 78 d1`                | Exchange Bluetooth LTK components                             |
| `0x0016` | `15 91 01 02 00 11 00 00 00 93 4e 58 0f 16 3a ee cf b5 75 fc 91 36 b2 2f bb`                | Exchange Bluetooth LTK confirm challenge/response             |
| `0x0016` | `15 91 01 03 00 01 00 00 00`                                                                | Finalise Bluetooth pairing                                    |
| `0x0016` | `03 91 01 07 00 16 00 00 5e 11 85 eb f1 48 c1 27 80 67 1a fd 29 b8 00 e1 dd c5 19 b4 f0 54` |                                                               |
| `0x0016` | `03 91 01 09 00 00 00 00`                                                                   |                                                               |
| `0x0016` | `0a 91 01 02 00 04 00 00 03 00 00 00`                                                       | Play connection vibration sample                              |
| `0x0016` | `09 91 01 07 00 08 00 00 01 00 00 00 00 00 00 00`                                           | Set player LEDs                                               |
| `0x0016` | `0c 91 01 02 00 04 00 00 37 00 00 00`                                                       | Initialise feature flags (`0x37`)                             |
| `0x0016` | `02 91 01 04 00 08 00 00 40 7e 00 00 80 30 01 00`                                           | Read `0x40` bytes from `0x13080`                              |
| `0x0016` | `02 91 01 04 00 08 00 00 40 7e 00 00 40 c0 1f 00`                                           | Read `0x40` bytes from `0x1fC040`                             |
| `0x0016` | `02 91 01 04 00 08 00 00 10 7e 00 00 40 30 01 00`                                           | Read `0x10` bytes from `0x13040`                              |
| `0x0016` | `02 91 01 04 00 08 00 00 18 7e 00 00 00 31 01 00`                                           | Read `0x18` bytes from `0x13100`                              |
| `0x0016` | `11 91 01 03 00 00 00 00`                                                                   |                                                               |
| `0x0016` | `02 91 01 04 00 08 00 00 20 7e 00 00 60 30 01 00`                                           | Read `0x20` bytes from `0x13060`                              |
| `0x0016` | `0a 91 01 08 00 14 00 00 01 59 09 00 00 ff ff ff ff 35 00 46 00 00 00 00 00 00 00 00`       | Send vibration data                                           |
| `0x0016` | `11 91 01 01 00 00 00 00`                                                                   |                                                               |
| `0x0016` | `0c 91 01 04 00 04 00 00 37 00 00 00`                                                       | Confirm feature flags (`0x37`)                                |
| `0x0010` | `85 00`                                                                                     |                                                               |
| `0x000F` | `01 00`                                                                                     | Enable notifications for HID input reports on handle `0x000E` |

#### NSO Gamecube Controller

| Handle   | Example Data                                                                                | Comment                                                       |
| ---      | ---                                                                                         | ---                                                           |
| `0x0005` | `01 00`                                                                                     |                                                               |
| `0x001B` | `01 00`                                                                                     | Enable notifications for command responses on handle `0x001A` |
| `0x001F` | `01 00`                                                                                     | Enable notifications for command responses on handle `0x001E` |
| `0x0023` | `01 00`                                                                                     | Enable notifications for command responses on handle `0x0022` |
| `0x0016` | `07 91 01 01 00 00 00 00`                                                                   |                                                               |
| `0x0016` | `02 91 01 04 00 08 00 00 40 7e 00 00 00 30 01 00`                                           | Read `0x40` bytes from `0x13000`                              |
| `0x0016` | `10 91 01 01 00 00 00 00`                                                                   |                                                               |
| `0x0016` | `16 91 01 01 00 00 00 00`                                                                   |                                                               |
| `0x0016` | `15 91 01 01 00 0e 00 00 00 02 81 eb 3a eb f1 48 80 eb 3a eb f1 48`                         | Exchange Bluetooth addresses                                  |
| `0x0016` | `15 91 01 04 00 11 00 00 00 35 03 e9 29 82 87 71 24 be a8 0c 66 46 15 83 4b`                | Exchange Bluetooth LTK components                             |
| `0x0016` | `15 01 01 02 10 78 00 00 01 13 4c 97 f5 11 b9 b6 dd 4d 86 fd 40 f5 36 e9 ed`                | Exchange Bluetooth LTK confirm challenge/response             |
| `0x0016` | `15 91 01 03 00 01 00 00 00`                                                                | Finalise Bluetooth pairing                                    |
| `0x0016` | `0a 91 01 02 00 04 00 00 03 00 00 00`                                                       | Play connection vibration sample                              |
| `0x0016` | `09 91 01 07 00 08 00 00 01 00 00 00 00 00 00 00`                                           | Set player LEDs                                               |
| `0x0016` | `0c 91 01 02 00 04 00 00 27 00 00 00`                                                       | Initialise feature flags (`0x27`)                             |
| `0x0016` | `02 91 01 04 00 08 00 00 40 7e 00 00 80 30 01 00`                                           | Read `0x40` bytes from `0x13080`                              |
| `0x0016` | `02 91 01 04 00 08 00 00 40 7e 00 00 c0 30 01 00`                                           | Read 0x40 bytes from `0x130C0`                                |
| `0x0016` | `02 91 01 04 00 08 00 00 40 7e 00 00 40 c0 1f 00`                                           | Read `0x40` bytes from `0x1fC040`                             |
| `0x0016` | `02 91 01 04 00 08 00 00 10 7e 00 00 40 30 01 00`                                           | Read `0x10` bytes from `0x13040`                              |
| `0x0016` | `02 91 01 04 00 08 00 00 18 7e 00 00 00 31 01 00`                                           | Read `0x18` bytes from `0x13100`                              |
| `0x0016` | `11 91 01 03 00 00 00 00`                                                                   |                                                               |
| `0x0016` | `02 91 01 04 00 08 00 00 02 7e 00 00 40 31 01 00`                                           | Read `0x02` bytes from `0x13140`                              |
| `0x0016` | `02 91 01 04 00 08 00 00 20 7e 00 00 60 30 01 00`                                           | Read `0x20` bytes from `0x13060`                              |
| `0x0016` | `0a 91 01 08 00 14 00 00 01 ff ff ff ff ff ff ff ff 35 00 46 00 00 00 00 00 00 00 00`       | Send vibration data                                           |
| `0x0016` | `0c 91 01 04 00 04 00 00 27 00 00 00`                                                       | Confirm feature flags (`0x27`)                                |
| `0x0010` | `85 00`                                                                                     |                                                               |
| `0x000F` | `01 00`                                                                                     | Enable notifications for HID input reports on handle `0x000E` |
