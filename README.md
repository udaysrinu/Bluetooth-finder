# Bluetooth-finder

Scans for nearby Bluetooth devices and lists each with its name (or MAC address) and RSSI.

> Built in 2020 during college as a learning project. Not actively maintained.

## What it does

Tap "Search" and the app kicks off a classic Bluetooth discovery via `BluetoothAdapter.startDiscovery()`. Each device the OS reports is added to a list as `name - RSSI XdBm` (or just MAC address if the name is null). The status text flips to "Searching..." while the scan runs and "Finished" when discovery completes. Designed as a starting point for any flow that needs to enumerate nearby BT devices.

## Tech stack

| Piece | Detail |
|---|---|
| Language | Java |
| compileSdk / minSdk / targetSdk | 26 / 22 / 26 |
| UI | `ListView` + `ArrayAdapter` |
| Bluetooth | `android.bluetooth.BluetoothAdapter`, classic Bluetooth discovery |
| Support libs | `com.android.support:appcompat-v7:26.1.0` (pre-AndroidX) |

## Notable techniques

- A single anonymous `BroadcastReceiver` handles both `BluetoothDevice.ACTION_FOUND` (a device showed up) and `BluetoothAdapter.ACTION_DISCOVERY_FINISHED` (scan ended). The receiver pulls `EXTRA_RSSI` as a `short` to populate the signal strength.
- The `addresses` list deduplicates devices by MAC, so the same device discovered twice during a scan doesn't double up in the UI.

## How to run

1. Open in Android Studio 3.x or 4.x. Gradle wrapper included.
2. Run on a real device — the emulator doesn't have a Bluetooth radio. API 22+.
3. The current code does not request `BLUETOOTH` / `BLUETOOTH_ADMIN` / `ACCESS_FINE_LOCATION` permissions at runtime, so on a modern device you'll need to grant them manually via Settings before discovery returns results.

## What I'd do differently today

- Migrate to AndroidX and modern Bluetooth permissions (`BLUETOOTH_SCAN`, `BLUETOOTH_CONNECT` on Android 12+).
- Request `ACCESS_FINE_LOCATION` at runtime — required for BT discovery on Android 6+, missing here.
- Unregister the `BroadcastReceiver` in `onDestroy` to avoid a leak.

## License

No license specified.
