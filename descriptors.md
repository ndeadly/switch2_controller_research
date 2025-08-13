# Descriptors

Below are the various descriptors for each device, obtained via USB and parsed using https://eleccelerator.com/usbdescreqparser/ and wireshark.

---

## JoyCon 2 (R)

#### USB Device Descriptor
```
0x12,        // bLength
0x01,        // bDescriptorType (Device)
0x00, 0x02,  // bcdUSB 2.00
0xEF,        // bDeviceClass 
0x02,        // bDeviceSubClass 
0x01,        // bDeviceProtocol 
0x40,        // bMaxPacketSize0 64
0x7E, 0x05,  // idVendor 0x057E
0x66, 0x20,  // idProduct 0x2066
0x00, 0x01,  // bcdDevice 2.00
0x01,        // iManufacturer (String Index)
0x02,        // iProduct (String Index)
0x03,        // iSerialNumber (String Index)
0x01,        // bNumConfigurations 1
```

#### USB Configuration Descriptor
```
0x09,        // bLength
0x02,        // bDescriptorType (Configuration)
0x50, 0x00,  // wTotalLength 80
0x02,        // bNumInterfaces 2
0x01,        // bConfigurationValue
0x04,        // iConfiguration (String Index)
0xC0,        // bmAttributes Self Powered
0xFA,        // bMaxPower 500mA

0x08,        // bLength
0x0B,        // bDescriptorType (Interface Association)
0x00,        // bFirstInterface
0x01,        // bInterfaceCount
0x03,        // bFunctionClass
0x00,        // bFunctionSubClass
0x00,        // bFunctionProtocol
0x00,        // iFunction

0x09,        // bLength
0x04,        // bDescriptorType (Interface)
0x00,        // bInterfaceNumber 0
0x00,        // bAlternateSetting
0x02,        // bNumEndpoints 2
0x03,        // bInterfaceClass
0x00,        // bInterfaceSubClass
0x00,        // bInterfaceProtocol
0x05,        // iInterface (String Index)

0x09,        // bLength
0x21,        // bDescriptorType (HID)
0x11, 0x01,  // bcdHID 1.11
0x00,        // bCountryCode
0x01,        // bNumDescriptors
0x22,        // bDescriptorType[0] (HID)
0x64, 0x00,  // wDescriptorLength[0] 100

0x07,        // bLength
0x05,        // bDescriptorType (Endpoint)
0x81,        // bEndpointAddress (IN/D2H)
0x03,        // bmAttributes (Interrupt)
0x40, 0x00,  // wMaxPacketSize 64
0x04,        // bInterval 4 (unit depends on device speed)

0x07,        // bLength
0x05,        // bDescriptorType (Endpoint)
0x01,        // bEndpointAddress (OUT/H2D)
0x03,        // bmAttributes (Interrupt)
0x40, 0x00,  // wMaxPacketSize 64
0x04,        // bInterval 4 (unit depends on device speed)

0x08,        // bLength
0x0B,        // bDescriptorType (Interface Association)
0x01,        // bFirstInterface
0x01,        // bInterfaceCount
0xFF,        // bFunctionClass
0x00,        // bFunctionSubClass
0x00,        // bFunctionProtocol
0x00,        // iFunction

0x09,        // bLength
0x04,        // bDescriptorType (Interface)
0x01,        // bInterfaceNumber 1
0x00,        // bAlternateSetting
0x02,        // bNumEndpoints 2
0xFF,        // bInterfaceClass
0x00,        // bInterfaceSubClass
0x00,        // bInterfaceProtocol
0x06,        // iInterface (String Index)

0x07,        // bLength
0x05,        // bDescriptorType (Endpoint)
0x02,        // bEndpointAddress (OUT/H2D)
0x02,        // bmAttributes (Bulk)
0x40, 0x00,  // wMaxPacketSize 64
0x00,        // bInterval 0 (unit depends on device speed)

0x07,        // bLength
0x05,        // bDescriptorType (Endpoint)
0x82,        // bEndpointAddress (IN/D2H)
0x02,        // bmAttributes (Bulk)
0x40, 0x00,  // wMaxPacketSize 64
0x00,        // bInterval 0 (unit depends on device speed)
```

#### Manufacturer String Descriptor
`Nintendo`

#### Product String Descriptor
`Joy-Con 2 (R)`

#### Serial String Descriptor
`00`

#### HID Report Descriptor
```
0x05, 0x01,        // Usage Page (Generic Desktop Ctrls)
0x09, 0x05,        // Usage (Game Pad)
0xA1, 0x01,        // Collection (Application)
0x85, 0x05,        //   Report ID (5)
0x05, 0xFF,        //   Usage Page (Reserved 0xFF)
0x09, 0x01,        //   Usage (0x01)
0x15, 0x00,        //   Logical Minimum (0)
0x26, 0xFF, 0x00,  //   Logical Maximum (255)
0x95, 0x3F,        //   Report Count (63)
0x75, 0x08,        //   Report Size (8)
0x81, 0x02,        //   Input (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position)
0x85, 0x08,        //   Report ID (8)
0x09, 0x01,        //   Usage (0x01)
0x95, 0x02,        //   Report Count (2)
0x81, 0x02,        //   Input (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position)
0x05, 0x09,        //   Usage Page (Button)
0x19, 0x01,        //   Usage Minimum (0x01)
0x29, 0x10,        //   Usage Maximum (0x10)
0x25, 0x01,        //   Logical Maximum (1)
0x95, 0x10,        //   Report Count (16)
0x75, 0x01,        //   Report Size (1)
0x81, 0x02,        //   Input (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position)
0x05, 0xFF,        //   Usage Page (Reserved 0xFF)
0x09, 0x01,        //   Usage (0x01)
0x26, 0xFF, 0x00,  //   Logical Maximum (255)
0x95, 0x01,        //   Report Count (1)
0x75, 0x08,        //   Report Size (8)
0x81, 0x02,        //   Input (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position)
0x05, 0x01,        //   Usage Page (Generic Desktop Ctrls)
0x09, 0x01,        //   Usage (Pointer)
0xA1, 0x00,        //   Collection (Physical)
0x09, 0x30,        //     Usage (X)
0x09, 0x31,        //     Usage (Y)
0x26, 0xFF, 0x0F,  //     Logical Maximum (4095)
0x95, 0x02,        //     Report Count (2)
0x75, 0x0C,        //     Report Size (12)
0x81, 0x02,        //     Input (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position)
0xC0,              //   End Collection
0x05, 0xFF,        //   Usage Page (Reserved 0xFF)
0x09, 0x02,        //   Usage (0x02)
0x26, 0xFF, 0x00,  //   Logical Maximum (255)
0x95, 0x37,        //   Report Count (55)
0x75, 0x08,        //   Report Size (8)
0x81, 0x02,        //   Input (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position)
0x85, 0x01,        //   Report ID (1)
0x09, 0x01,        //   Usage (0x01)
0x95, 0x3F,        //   Report Count (63)
0x91, 0x02,        //   Output (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position,Non-volatile)
0xC0,              // End Collection
```

---

## JoyCon 2 (L)

#### USB Device Descriptor
```
0x12,        // bLength
0x01,        // bDescriptorType (Device)
0x00, 0x02,  // bcdUSB 2.00
0xEF,        // bDeviceClass 
0x02,        // bDeviceSubClass 
0x01,        // bDeviceProtocol 
0x40,        // bMaxPacketSize0 64
0x7E, 0x05,  // idVendor 0x057E
0x67, 0x20,  // idProduct 0x2067
0x00, 0x01,  // bcdDevice 2.00
0x01,        // iManufacturer (String Index)
0x02,        // iProduct (String Index)
0x03,        // iSerialNumber (String Index)
0x01,        // bNumConfigurations 1
```

#### USB Configuration Descriptor
```
0x09,        // bLength
0x02,        // bDescriptorType (Configuration)
0x50, 0x00,  // wTotalLength 80
0x02,        // bNumInterfaces 2
0x01,        // bConfigurationValue
0x04,        // iConfiguration (String Index)
0xC0,        // bmAttributes Self Powered
0xFA,        // bMaxPower 500mA

0x08,        // bLength
0x0B,        // bDescriptorType (Interface Association)
0x00,        // bFirstInterface
0x01,        // bInterfaceCount
0x03,        // bFunctionClass
0x00,        // bFunctionSubClass
0x00,        // bFunctionProtocol
0x00,        // iFunction

0x09,        // bLength
0x04,        // bDescriptorType (Interface)
0x00,        // bInterfaceNumber 0
0x00,        // bAlternateSetting
0x02,        // bNumEndpoints 2
0x03,        // bInterfaceClass
0x00,        // bInterfaceSubClass
0x00,        // bInterfaceProtocol
0x05,        // iInterface (String Index)

0x09,        // bLength
0x21,        // bDescriptorType (HID)
0x11, 0x01,  // bcdHID 1.11
0x00,        // bCountryCode
0x01,        // bNumDescriptors
0x22,        // bDescriptorType[0] (HID)
0x64, 0x00,  // wDescriptorLength[0] 100

0x07,        // bLength
0x05,        // bDescriptorType (Endpoint)
0x81,        // bEndpointAddress (IN/D2H)
0x03,        // bmAttributes (Interrupt)
0x40, 0x00,  // wMaxPacketSize 64
0x04,        // bInterval 4 (unit depends on device speed)

0x07,        // bLength
0x05,        // bDescriptorType (Endpoint)
0x01,        // bEndpointAddress (OUT/H2D)
0x03,        // bmAttributes (Interrupt)
0x40, 0x00,  // wMaxPacketSize 64
0x04,        // bInterval 4 (unit depends on device speed)

0x08,        // bLength
0x0B,        // bDescriptorType (Interface Association)
0x01,        // bFirstInterface
0x01,        // bInterfaceCount
0xFF,        // bFunctionClass
0x00,        // bFunctionSubClass
0x00,        // bFunctionProtocol
0x00,        // iFunction

0x09,        // bLength
0x04,        // bDescriptorType (Interface)
0x01,        // bInterfaceNumber 1
0x00,        // bAlternateSetting
0x02,        // bNumEndpoints 2
0xFF,        // bInterfaceClass
0x00,        // bInterfaceSubClass
0x00,        // bInterfaceProtocol
0x06,        // iInterface (String Index)

0x07,        // bLength
0x05,        // bDescriptorType (Endpoint)
0x02,        // bEndpointAddress (OUT/H2D)
0x02,        // bmAttributes (Bulk)
0x40, 0x00,  // wMaxPacketSize 64
0x00,        // bInterval 0 (unit depends on device speed)

0x07,        // bLength
0x05,        // bDescriptorType (Endpoint)
0x82,        // bEndpointAddress (IN/D2H)
0x02,        // bmAttributes (Bulk)
0x40, 0x00,  // wMaxPacketSize 64
0x00,        // bInterval 0 (unit depends on device speed)
```

#### Manufacturer String Descriptor
`Nintendo`

#### Product String Descriptor
`Joy-Con 2 (L)`

#### Serial String Descriptor
`00`

#### HID Report Descriptor
```
0x05, 0x01,        // Usage Page (Generic Desktop Ctrls)
0x09, 0x05,        // Usage (Game Pad)
0xA1, 0x01,        // Collection (Application)
0x85, 0x05,        //   Report ID (5)
0x05, 0xFF,        //   Usage Page (Reserved 0xFF)
0x09, 0x01,        //   Usage (0x01)
0x15, 0x00,        //   Logical Minimum (0)
0x26, 0xFF, 0x00,  //   Logical Maximum (255)
0x95, 0x3F,        //   Report Count (63)
0x75, 0x08,        //   Report Size (8)
0x81, 0x02,        //   Input (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position)
0x85, 0x07,        //   Report ID (7)
0x09, 0x01,        //   Usage (0x01)
0x95, 0x02,        //   Report Count (2)
0x81, 0x02,        //   Input (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position)
0x05, 0x09,        //   Usage Page (Button)
0x19, 0x01,        //   Usage Minimum (0x01)
0x29, 0x10,        //   Usage Maximum (0x10)
0x25, 0x01,        //   Logical Maximum (1)
0x95, 0x10,        //   Report Count (16)
0x75, 0x01,        //   Report Size (1)
0x81, 0x02,        //   Input (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position)
0x05, 0xFF,        //   Usage Page (Reserved 0xFF)
0x09, 0x01,        //   Usage (0x01)
0x26, 0xFF, 0x00,  //   Logical Maximum (255)
0x95, 0x01,        //   Report Count (1)
0x75, 0x08,        //   Report Size (8)
0x81, 0x02,        //   Input (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position)
0x05, 0x01,        //   Usage Page (Generic Desktop Ctrls)
0x09, 0x01,        //   Usage (Pointer)
0xA1, 0x00,        //   Collection (Physical)
0x09, 0x30,        //     Usage (X)
0x09, 0x31,        //     Usage (Y)
0x26, 0xFF, 0x0F,  //     Logical Maximum (4095)
0x95, 0x02,        //     Report Count (2)
0x75, 0x0C,        //     Report Size (12)
0x81, 0x02,        //     Input (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position)
0xC0,              //   End Collection
0x05, 0xFF,        //   Usage Page (Reserved 0xFF)
0x09, 0x02,        //   Usage (0x02)
0x26, 0xFF, 0x00,  //   Logical Maximum (255)
0x95, 0x37,        //   Report Count (55)
0x75, 0x08,        //   Report Size (8)
0x81, 0x02,        //   Input (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position)
0x85, 0x01,        //   Report ID (1)
0x09, 0x01,        //   Usage (0x01)
0x95, 0x3F,        //   Report Count (63)
0x91, 0x02,        //   Output (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position,Non-volatile)
0xC0,              // End Collection
```

---

## Pro Controller 2

#### USB Device Descriptor
```
0x12,        // bLength
0x01,        // bDescriptorType (Device)
0x00, 0x02,  // bcdUSB 2.00
0xEF,        // bDeviceClass 
0x02,        // bDeviceSubClass 
0x01,        // bDeviceProtocol 
0x40,        // bMaxPacketSize0 64
0x7E, 0x05,  // idVendor 0x057E
0x69, 0x20,  // idProduct 0x2069
0x00, 0x02,  // bcdDevice 4.00
0x01,        // iManufacturer (String Index)
0x02,        // iProduct (String Index)
0x03,        // iSerialNumber (String Index)
0x01,        // bNumConfigurations 1
```

#### USB Configuration Descriptor
```
0x09,        // bLength
0x02,        // bDescriptorType (Configuration)
0x0C, 0x01,  // wTotalLength 268
0x05,        // bNumInterfaces 5
0x01,        // bConfigurationValue
0x04,        // iConfiguration (String Index)
0xC0,        // bmAttributes Self Powered
0xFA,        // bMaxPower 500mA

0x08,        // bLength
0x0B,        // bDescriptorType (Interface Association)
0x00,        // bFirstInterface
0x01,        // bInterfaceCount
0x03,        // bFunctionClass
0x00,        // bFunctionSubClass
0x00,        // bFunctionProtocol
0x00,        // iFunction

0x09,        // bLength
0x04,        // bDescriptorType (Interface)
0x00,        // bInterfaceNumber 0
0x00,        // bAlternateSetting
0x02,        // bNumEndpoints 2
0x03,        // bInterfaceClass
0x00,        // bInterfaceSubClass
0x00,        // bInterfaceProtocol
0x05,        // iInterface (String Index)

0x09,        // bLength
0x21,        // bDescriptorType (HID)
0x11, 0x01,  // bcdHID 1.11
0x00,        // bCountryCode
0x01,        // bNumDescriptors
0x22,        // bDescriptorType[0] (HID)
0x61, 0x00,  // wDescriptorLength[0] 97

0x07,        // bLength
0x05,        // bDescriptorType (Endpoint)
0x81,        // bEndpointAddress (IN/D2H)
0x03,        // bmAttributes (Interrupt)
0x40, 0x00,  // wMaxPacketSize 64
0x04,        // bInterval 4 (unit depends on device speed)

0x07,        // bLength
0x05,        // bDescriptorType (Endpoint)
0x01,        // bEndpointAddress (OUT/H2D)
0x03,        // bmAttributes (Interrupt)
0x40, 0x00,  // wMaxPacketSize 64
0x04,        // bInterval 4 (unit depends on device speed)

0x08,        // bLength
0x0B,        // bDescriptorType (Interface Association)
0x01,        // bFirstInterface
0x01,        // bInterfaceCount
0xFF,        // bFunctionClass
0x00,        // bFunctionSubClass
0x00,        // bFunctionProtocol
0x00,        // iFunction

0x09,        // bLength
0x04,        // bDescriptorType (Interface)
0x01,        // bInterfaceNumber 1
0x00,        // bAlternateSetting
0x02,        // bNumEndpoints 2
0xFF,        // bInterfaceClass
0x00,        // bInterfaceSubClass
0x00,        // bInterfaceProtocol
0x06,        // iInterface (String Index)

0x07,        // bLength
0x05,        // bDescriptorType (Endpoint)
0x02,        // bEndpointAddress (OUT/H2D)
0x02,        // bmAttributes (Bulk)
0x40, 0x00,  // wMaxPacketSize 64
0x00,        // bInterval 0 (unit depends on device speed)

0x07,        // bLength
0x05,        // bDescriptorType (Endpoint)
0x82,        // bEndpointAddress (IN/D2H)
0x02,        // bmAttributes (Bulk)
0x40, 0x00,  // wMaxPacketSize 64
0x00,        // bInterval 0 (unit depends on device speed)

0x08,        // bLength
0x0B,        // bDescriptorType (Interface Association)
0x02,        // bFirstInterface
0x03,        // bInterfaceCount
0x01,        // bFunctionClass
0x01,        // bFunctionSubClass
0x00,        // bFunctionProtocol
0x00,        // iFunction

0x09,        // bLength
0x04,        // bDescriptorType (Interface)
0x02,        // bInterfaceNumber 2
0x00,        // bAlternateSetting
0x00,        // bNumEndpoints 0
0x01,        // bInterfaceClass (Audio)
0x01,        // bInterfaceSubClass (Audio Control)
0x00,        // bInterfaceProtocol
0x00,        // iInterface (String Index)

0x0A,        // bLength
0x24,        // bDescriptorType (See Next Line)
0x01,        // bDescriptorSubtype (CS_INTERFACE -> HEADER)
0x00, 0x01,  // bcdADC 1.00
0x47, 0x00,  // wTotalLength 71
0x02,        // binCollection 0x02
0x03,        // baInterfaceNr 3
0x04,        // baInterfaceNr 4

0x0C,        // bLength
0x24,        // bDescriptorType (See Next Line)
0x02,        // bDescriptorSubtype (CS_INTERFACE -> INPUT_TERMINAL)
0x01,        // bTerminalID
0x01, 0x01,  // wTerminalType (USB Streaming)
0x00,        // bAssocTerminal
0x02,        // bNrChannels 2
0x03, 0x00,  // wChannelConfig (Left and Right Front)
0x00,        // iChannelNames
0x00,        // iTerminal

0x0A,        // bLength
0x24,        // bDescriptorType (See Next Line)
0x06,        // bDescriptorSubtype (CS_INTERFACE -> FEATURE_UNIT)
0x02,        // bUnitID
0x01,        // bSourceID
0x01,        // bControlSize 1
0x03, 0x00,  // bmaControls[0] (Mute,Volume)
0x00, 0x00,  // bmaControls[1] (None)

0x09,        // bLength
0x24,        // bDescriptorType (See Next Line)
0x03,        // bDescriptorSubtype (CS_INTERFACE -> OUTPUT_TERMINAL)
0x03,        // bTerminalID
0x02, 0x03,  // wTerminalType (Headphones)
0x00,        // bAssocTerminal
0x02,        // bSourceID
0x00,        // iTerminal

0x0C,        // bLength
0x24,        // bDescriptorType (See Next Line)
0x02,        // bDescriptorSubtype (CS_INTERFACE -> INPUT_TERMINAL)
0x04,        // bTerminalID
0x01, 0x02,  // wTerminalType (Microphone)
0x00,        // bAssocTerminal
0x01,        // bNrChannels 1
0x00, 0x00,  // wChannelConfig
0x00,        // iChannelNames
0x00,        // iTerminal

0x09,        // bLength
0x24,        // bDescriptorType (See Next Line)
0x06,        // bDescriptorSubtype (CS_INTERFACE -> FEATURE_UNIT)
0x05,        // bUnitID
0x04,        // bSourceID
0x01,        // bControlSize 1
0x03, 0x00,  // bmaControls[0] (Mute,Volume)
0x00,        // iFeature

0x09,        // bLength
0x24,        // bDescriptorType (See Next Line)
0x03,        // bDescriptorSubtype (CS_INTERFACE -> OUTPUT_TERMINAL)
0x06,        // bTerminalID
0x01, 0x01,  // wTerminalType (USB Streaming)
0x00,        // bAssocTerminal
0x05,        // bSourceID
0x00,        // iTerminal

0x09,        // bLength
0x04,        // bDescriptorType (Interface)
0x03,        // bInterfaceNumber 3
0x00,        // bAlternateSetting
0x00,        // bNumEndpoints 0
0x01,        // bInterfaceClass (Audio)
0x02,        // bInterfaceSubClass (Audio Streaming)
0x00,        // bInterfaceProtocol
0x00,        // iInterface (String Index)

0x09,        // bLength
0x04,        // bDescriptorType (Interface)
0x03,        // bInterfaceNumber 3
0x01,        // bAlternateSetting
0x01,        // bNumEndpoints 1
0x01,        // bInterfaceClass (Audio)
0x02,        // bInterfaceSubClass (Audio Streaming)
0x00,        // bInterfaceProtocol
0x00,        // iInterface (String Index)

0x07,        // bLength
0x24,        // bDescriptorType (See Next Line)
0x01,        // bDescriptorSubtype (CS_INTERFACE -> AS_GENERAL)
0x01,        // bTerminalLink
0x00,        // bDelay 0
0x01, 0x00,  // wFormatTag (PCM)

0x0B,        // bLength
0x24,        // bDescriptorType (See Next Line)
0x02,        // bDescriptorSubtype (CS_INTERFACE -> FORMAT_TYPE)
0x01,        // bFormatType 1
0x02,        // bNrChannels (Stereo)
0x02,        // bSubFrameSize 2
0x10,        // bBitResolution 16
0x01,        // bSamFreqType 1
0x80, 0xBB, 0x00,  // tSamFreq[1] 48000 Hz

0x07,        // bLength
0x05,        // bDescriptorType (See Next Line)
0x03,        // bEndpointAddress (OUT/H2D)
0x0D,        // bmAttributes (Isochronous, Sync, Data EP)
0xC0, 0x00,  // wMaxPacketSize 192
0x01,        // bInterval 1 (unit depends on device speed)

0x07,        // bLength
0x25,        // bDescriptorType (See Next Line)
0x01,        // bDescriptorSubtype (CS_ENDPOINT -> EP_GENERAL)
0x00,        // bmAttributes (None)
0x00,        // bLockDelayUnits
0x00, 0x00,  // wLockDelay 0

0x09,        // bLength
0x04,        // bDescriptorType (Interface)
0x04,        // bInterfaceNumber 4
0x00,        // bAlternateSetting
0x00,        // bNumEndpoints 0
0x01,        // bInterfaceClass (Audio)
0x02,        // bInterfaceSubClass (Audio Streaming)
0x00,        // bInterfaceProtocol
0x00,        // iInterface (String Index)

0x09,        // bLength
0x04,        // bDescriptorType (Interface)
0x04,        // bInterfaceNumber 4
0x01,        // bAlternateSetting
0x01,        // bNumEndpoints 1
0x01,        // bInterfaceClass (Audio)
0x02,        // bInterfaceSubClass (Audio Streaming)
0x00,        // bInterfaceProtocol
0x00,        // iInterface (String Index)

0x07,        // bLength
0x24,        // bDescriptorType (See Next Line)
0x01,        // bDescriptorSubtype (CS_INTERFACE -> AS_GENERAL)
0x06,        // bTerminalLink
0x00,        // bDelay 0
0x01, 0x00,  // wFormatTag (PCM)

0x0B,        // bLength
0x24,        // bDescriptorType (See Next Line)
0x02,        // bDescriptorSubtype (CS_INTERFACE -> FORMAT_TYPE)
0x01,        // bFormatType 1
0x02,        // bNrChannels (Stereo)
0x02,        // bSubFrameSize 2
0x10,        // bBitResolution 16
0x01,        // bSamFreqType 1
0x80, 0xBB, 0x00,  // tSamFreq[1] 48000 Hz

0x07,        // bLength
0x05,        // bDescriptorType (See Next Line)
0x83,        // bEndpointAddress (IN/D2H)
0x0D,        // bmAttributes (Isochronous, Sync, Data EP)
0xC0, 0x00,  // wMaxPacketSize 192
0x01,        // bInterval 1 (unit depends on device speed)

0x07,        // bLength
0x25,        // bDescriptorType (See Next Line)
0x01,        // bDescriptorSubtype (CS_ENDPOINT -> EP_GENERAL)
0x00,        // bmAttributes (None)
0x00,        // bLockDelayUnits
0x00, 0x00,  // wLockDelay 0
```

#### Manufacturer String Descriptor
`Nintendo`

#### Product String Descriptor
`Switch 2 Pro Controller`

#### Serial String Descriptor
`00`

#### HID Report Descriptor
```
0x05, 0x01,        // Usage Page (Generic Desktop Ctrls)
0x09, 0x05,        // Usage (Game Pad)
0xA1, 0x01,        // Collection (Application)
0x85, 0x05,        //   Report ID (5)
0x05, 0xFF,        //   Usage Page (Reserved 0xFF)
0x09, 0x01,        //   Usage (0x01)
0x15, 0x00,        //   Logical Minimum (0)
0x26, 0xFF, 0x00,  //   Logical Maximum (255)
0x95, 0x3F,        //   Report Count (63)
0x75, 0x08,        //   Report Size (8)
0x81, 0x02,        //   Input (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position)
0x85, 0x09,        //   Report ID (9)
0x09, 0x01,        //   Usage (0x01)
0x95, 0x02,        //   Report Count (2)
0x81, 0x02,        //   Input (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position)
0x05, 0x09,        //   Usage Page (Button)
0x19, 0x01,        //   Usage Minimum (0x01)
0x29, 0x15,        //   Usage Maximum (0x15)
0x25, 0x01,        //   Logical Maximum (1)
0x95, 0x15,        //   Report Count (21)
0x75, 0x01,        //   Report Size (1)
0x81, 0x02,        //   Input (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position)
0x95, 0x01,        //   Report Count (1)
0x75, 0x03,        //   Report Size (3)
0x81, 0x03,        //   Input (Const,Var,Abs,No Wrap,Linear,Preferred State,No Null Position)
0x05, 0x01,        //   Usage Page (Generic Desktop Ctrls)
0x09, 0x01,        //   Usage (Pointer)
0xA1, 0x00,        //   Collection (Physical)
0x09, 0x30,        //     Usage (X)
0x09, 0x31,        //     Usage (Y)
0x09, 0x33,        //     Usage (Rx)
0x09, 0x35,        //     Usage (Rz)
0x26, 0xFF, 0x0F,  //     Logical Maximum (4095)
0x95, 0x04,        //     Report Count (4)
0x75, 0x0C,        //     Report Size (12)
0x81, 0x02,        //     Input (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position)
0xC0,              //   End Collection
0x05, 0xFF,        //   Usage Page (Reserved 0xFF)
0x09, 0x02,        //   Usage (0x02)
0x26, 0xFF, 0x00,  //   Logical Maximum (255)
0x95, 0x34,        //   Report Count (52)
0x75, 0x08,        //   Report Size (8)
0x81, 0x02,        //   Input (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position)
0x85, 0x02,        //   Report ID (2)
0x09, 0x01,        //   Usage (0x01)
0x95, 0x3F,        //   Report Count (63)
0x91, 0x02,        //   Output (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position,Non-volatile)
0xC0,              // End Collection
```

---

## NSO Gamecube Controller

#### USB Device Descriptor
```
0x12,        // bLength
0x01,        // bDescriptorType (Device)
0x00, 0x02,  // bcdUSB 2.00
0xEF,        // bDeviceClass 
0x02,        // bDeviceSubClass 
0x01,        // bDeviceProtocol 
0x40,        // bMaxPacketSize0 64
0x7E, 0x05,  // idVendor 0x057E
0x73, 0x20,  // idProduct 0x2073
0x01, 0x01,  // bcdDevice 2.01
0x01,        // iManufacturer (String Index)
0x02,        // iProduct (String Index)
0x03,        // iSerialNumber (String Index)
0x01,        // bNumConfigurations 1
```

#### USB Configuration Descriptor
```
0x09,        // bLength
0x02,        // bDescriptorType (Configuration)
0x50, 0x00,  // wTotalLength 80
0x02,        // bNumInterfaces 2
0x01,        // bConfigurationValue
0x04,        // iConfiguration (String Index)
0xC0,        // bmAttributes Self Powered
0xFA,        // bMaxPower 500mA

0x08,        // bLength
0x0B,        // bDescriptorType (Interface Association)
0x00,        // bFirstInterface
0x01,        // bInterfaceCount
0x03,        // bFunctionClass
0x00,        // bFunctionSubClass
0x00,        // bFunctionProtocol
0x00,        // iFunction
 
0x09,        // bLength
0x04,        // bDescriptorType (Interface)
0x00,        // bInterfaceNumber 0
0x00,        // bAlternateSetting
0x02,        // bNumEndpoints 2
0x03,        // bInterfaceClass
0x00,        // bInterfaceSubClass
0x00,        // bInterfaceProtocol
0x05,        // iInterface (String Index)

0x09,        // bLength
0x21,        // bDescriptorType (HID)
0x11, 0x01,  // bcdHID 1.11
0x00,        // bCountryCode
0x01,        // bNumDescriptors
0x22,        // bDescriptorType[0] (HID)
0x61, 0x00,  // wDescriptorLength[0] 97

0x07,        // bLength
0x05,        // bDescriptorType (Endpoint)
0x81,        // bEndpointAddress (IN/D2H)
0x03,        // bmAttributes (Interrupt)
0x40, 0x00,  // wMaxPacketSize 64
0x04,        // bInterval 4 (unit depends on device speed)

0x07,        // bLength
0x05,        // bDescriptorType (Endpoint)
0x01,        // bEndpointAddress (OUT/H2D)
0x03,        // bmAttributes (Interrupt)
0x40, 0x00,  // wMaxPacketSize 64
0x04,        // bInterval 4 (unit depends on device speed)

0x08,        // bLength
0x0B,        // bDescriptorType (Interface Association)
0x01,        // bFirstInterface
0x01,        // bInterfaceCount
0xFF,        // bFunctionClass
0x00,        // bFunctionSubClass
0x00,        // bFunctionProtocol
0x00,        // iFunction

0x09,        // bLength
0x04,        // bDescriptorType (Interface)
0x01,        // bInterfaceNumber 1
0x00,        // bAlternateSetting
0x02,        // bNumEndpoints 2
0xFF,        // bInterfaceClass
0x00,        // bInterfaceSubClass
0x00,        // bInterfaceProtocol
0x06,        // iInterface (String Index)

0x07,        // bLength
0x05,        // bDescriptorType (Endpoint)
0x02,        // bEndpointAddress (OUT/H2D)
0x02,        // bmAttributes (Bulk)
0x40, 0x00,  // wMaxPacketSize 64
0x00,        // bInterval 0 (unit depends on device speed)

0x07,        // bLength
0x05,        // bDescriptorType (Endpoint)
0x82,        // bEndpointAddress (IN/D2H)
0x02,        // bmAttributes (Bulk)
0x40, 0x00,  // wMaxPacketSize 64
0x00,        // bInterval 0 (unit depends on device speed)
```

#### Manufacturer String Descriptor
`Nintendo`

#### Product String Descriptor
`Nintendo GameCube Controller`

#### Serial String Descriptor
`00`

#### HID Report Descriptor
```
0x05, 0x01,        // Usage Page (Generic Desktop Ctrls)
0x09, 0x05,        // Usage (Game Pad)
0xA1, 0x01,        // Collection (Application)
0x85, 0x05,        //   Report ID (5)
0x05, 0xFF,        //   Usage Page (Reserved 0xFF)
0x09, 0x01,        //   Usage (0x01)
0x15, 0x00,        //   Logical Minimum (0)
0x26, 0xFF, 0x00,  //   Logical Maximum (255)
0x95, 0x3F,        //   Report Count (63)
0x75, 0x08,        //   Report Size (8)
0x81, 0x02,        //   Input (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position)
0x85, 0x0A,        //   Report ID (10)
0x09, 0x01,        //   Usage (0x01)
0x95, 0x02,        //   Report Count (2)
0x81, 0x02,        //   Input (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position)
0x05, 0x09,        //   Usage Page (Button)
0x19, 0x01,        //   Usage Minimum (0x01)
0x29, 0x15,        //   Usage Maximum (0x15)
0x25, 0x01,        //   Logical Maximum (1)
0x95, 0x15,        //   Report Count (21)
0x75, 0x01,        //   Report Size (1)
0x81, 0x02,        //   Input (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position)
0x95, 0x01,        //   Report Count (1)
0x75, 0x03,        //   Report Size (3)
0x81, 0x03,        //   Input (Const,Var,Abs,No Wrap,Linear,Preferred State,No Null Position)
0x05, 0x01,        //   Usage Page (Generic Desktop Ctrls)
0x09, 0x01,        //   Usage (Pointer)
0xA1, 0x00,        //   Collection (Physical)
0x09, 0x30,        //     Usage (X)
0x09, 0x31,        //     Usage (Y)
0x09, 0x33,        //     Usage (Rx)
0x09, 0x35,        //     Usage (Rz)
0x26, 0xFF, 0x0F,  //     Logical Maximum (4095)
0x95, 0x04,        //     Report Count (4)
0x75, 0x0C,        //     Report Size (12)
0x81, 0x02,        //     Input (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position)
0xC0,              //   End Collection
0x05, 0xFF,        //   Usage Page (Reserved 0xFF)
0x09, 0x02,        //   Usage (0x02)
0x26, 0xFF, 0x00,  //   Logical Maximum (255)
0x95, 0x34,        //   Report Count (52)
0x75, 0x08,        //   Report Size (8)
0x81, 0x02,        //   Input (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position)
0x85, 0x03,        //   Report ID (3)
0x09, 0x01,        //   Usage (0x01)
0x95, 0x3F,        //   Report Count (63)
0x91, 0x02,        //   Output (Data,Var,Abs,No Wrap,Linear,Preferred State,No Null Position,Non-volatile)
0xC0,              // End Collection
```

---

## JoyCon 2 (R) (Safe Mode)

#### USB Device Descriptor
```
0x12,        // bLength
0x01,        // bDescriptorType (Device)
0x00, 0x02,  // bcdUSB 2.00
0xFF,        // bDeviceClass 
0x00,        // bDeviceSubClass 
0x01,        // bDeviceProtocol 
0x40,        // bMaxPacketSize0 64
0x7E, 0x05,  // idVendor 0x057E
0x70, 0x20,  // idProduct 0x2070
0x13, 0x01,  // bcdDevice 2.13
0x01,        // iManufacturer (String Index)
0x02,        // iProduct (String Index)
0x03,        // iSerialNumber (String Index)
0x01,        // bNumConfigurations 1
```

#### USB Configuration Descriptor
```
0x09,        // bLength
0x02,        // bDescriptorType (Configuration)
0x20, 0x00,  // wTotalLength 32
0x01,        // bNumInterfaces 1
0x01,        // bConfigurationValue
0x04,        // iConfiguration (String Index)
0xC0,        // bmAttributes Self Powered
0xFA,        // bMaxPower 500mA

0x09,        // bLength
0x04,        // bDescriptorType (Interface)
0x00,        // bInterfaceNumber 0
0x00,        // bAlternateSetting
0x02,        // bNumEndpoints 2
0xFF,        // bInterfaceClass
0x00,        // bInterfaceSubClass
0x00,        // bInterfaceProtocol
0x05,        // iInterface (String Index)

0x07,        // bLength
0x05,        // bDescriptorType (Endpoint)
0x01,        // bEndpointAddress (OUT/H2D)
0x02,        // bmAttributes (Bulk)
0x40, 0x00,  // wMaxPacketSize 64
0x00,        // bInterval 0 (unit depends on device speed)

0x07,        // bLength
0x05,        // bDescriptorType (Endpoint)
0x81,        // bEndpointAddress (IN/D2H)
0x02,        // bmAttributes (Bulk)
0x40, 0x00,  // wMaxPacketSize 64
0x00,        // bInterval 0 (unit depends on device speed)
```

---

## JoyCon 2 (L) (Safe Mode)

#### USB Device Descriptor
```
0x12,        // bLength
0x01,        // bDescriptorType (Device)
0x00, 0x02,  // bcdUSB 2.00
0xFF,        // bDeviceClass 
0x00,        // bDeviceSubClass 
0x01,        // bDeviceProtocol 
0x40,        // bMaxPacketSize0 64
0x7E, 0x05,  // idVendor 0x057E
0x71, 0x20,  // idProduct 0x2071
0x13, 0x01,  // bcdDevice 2.13
0x01,        // iManufacturer (String Index)
0x02,        // iProduct (String Index)
0x03,        // iSerialNumber (String Index)
0x01,        // bNumConfigurations 1
```

#### USB Configuration Descriptor
```
0x09,        // bLength
0x02,        // bDescriptorType (Configuration)
0x20, 0x00,  // wTotalLength 32
0x01,        // bNumInterfaces 1
0x01,        // bConfigurationValue
0x04,        // iConfiguration (String Index)
0xC0,        // bmAttributes Self Powered
0xFA,        // bMaxPower 500mA

0x09,        // bLength
0x04,        // bDescriptorType (Interface)
0x00,        // bInterfaceNumber 0
0x00,        // bAlternateSetting
0x02,        // bNumEndpoints 2
0xFF,        // bInterfaceClass
0x00,        // bInterfaceSubClass
0x00,        // bInterfaceProtocol
0x05,        // iInterface (String Index)

0x07,        // bLength
0x05,        // bDescriptorType (Endpoint)
0x01,        // bEndpointAddress (OUT/H2D)
0x02,        // bmAttributes (Bulk)
0x40, 0x00,  // wMaxPacketSize 64
0x00,        // bInterval 0 (unit depends on device speed)

0x07,        // bLength
0x05,        // bDescriptorType (Endpoint)
0x81,        // bEndpointAddress (IN/D2H)
0x02,        // bmAttributes (Bulk)
0x40, 0x00,  // wMaxPacketSize 64
0x00,        // bInterval 0 (unit depends on device speed)
```

---

## Pro Controller 2 (Safe Mode)

#### USB Device Descriptor
```
0x12,        // bLength
0x01,        // bDescriptorType (Device)
0x00, 0x02,  // bcdUSB 2.00
0xFF,        // bDeviceClass 
0x00,        // bDeviceSubClass 
0x01,        // bDeviceProtocol 
0x40,        // bMaxPacketSize0 64
0x7E, 0x05,  // idVendor 0x057E
0x72, 0x20,  // idProduct 0x2072
0x13, 0x01,  // bcdDevice 2.13
0x01,        // iManufacturer (String Index)
0x02,        // iProduct (String Index)
0x03,        // iSerialNumber (String Index)
0x01,        // bNumConfigurations 1
```

#### USB Configuration Descriptor
```
0x09,        // bLength
0x02,        // bDescriptorType (Configuration)
0x20, 0x00,  // wTotalLength 32
0x01,        // bNumInterfaces 1
0x01,        // bConfigurationValue
0x04,        // iConfiguration (String Index)
0xC0,        // bmAttributes Self Powered
0xFA,        // bMaxPower 500mA

0x09,        // bLength
0x04,        // bDescriptorType (Interface)
0x00,        // bInterfaceNumber 0
0x00,        // bAlternateSetting
0x02,        // bNumEndpoints 2
0xFF,        // bInterfaceClass
0x00,        // bInterfaceSubClass
0x00,        // bInterfaceProtocol
0x05,        // iInterface (String Index)

0x07,        // bLength
0x05,        // bDescriptorType (Endpoint)
0x01,        // bEndpointAddress (OUT/H2D)
0x02,        // bmAttributes (Bulk)
0x40, 0x00,  // wMaxPacketSize 64
0x00,        // bInterval 0 (unit depends on device speed)

0x07,        // bLength
0x05,        // bDescriptorType (Endpoint)
0x81,        // bEndpointAddress (IN/D2H)
0x02,        // bmAttributes (Bulk)
0x40, 0x00,  // wMaxPacketSize 64
0x00,        // bInterval 0 (unit depends on device speed)
```

## NSO Gamecube Controller (Safe Mode)

#### USB Device Descriptor
```
```

#### USB Configuration Descriptor
```
```