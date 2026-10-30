# BTSmpFixup

Lilu plugin for the Realtek RTL8852BE Bluetooth controller on macOS Tahoe. It is loaded only when the boot argument `-btsmpforce` is present.

## Hardware

| Part | Model |
| --- | --- |
| Bluetooth controller | Realtek RTL8852BE |

## Install order

OpenCore loads `Kernel -> Add` from top to bottom. Lilu must be first. The two plugins depend on it.

1. **Lilu** 1.7.3 (`as.vit9696.Lilu`)
2. **RealtekBluetoothFirmware** 1.0.2 (`org.thegwchr.RealtekBluetoothFirmware`) — firmware for the RTL8852BE
3. **BlueToolFixup** 2.7.3 (`as.acidanthera.BlueToolFixup`) — `MinKernel` `21.0.0`
4. **BTSmpFixup** 2.7.3 (`as.local.BTSmpFixup`) — `MinKernel` `21.0.0`

Copy each bundle to `EFI/OC/Kexts/` and add it in that order. Leave the existing entries above Lilu untouched. Do not put BTSmpFixup ahead of Lilu.

Boot arguments used with this stack:

```
-btlfxallowanyaddr -btlfxboardid -btsmpforce
```

`-btsmpforce` turns the patch on. Without it the kext stays loaded and does not patch `bluetoothd`.

Reboot from that EFI. `kmutil showloaded` should list `as.local.BTSmpFixup (2.7.3)`.
