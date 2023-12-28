# Galaxy Watch4 / Android Notes

Project for creating this Galaxy Watch 4 face.

![Alt text](images/dev1.png?raw=true "Watch Face Studio screenshot")

On Linux host, connect to wifi network to connect to phone/watch, which are then available on both Linux and Windows VM.

## Connecting to watch

Enable debug mode on watch:

- Settings>About>Software Information> tap Software version six times
- Enable ADB debug mode and wifi debugging
- Enter pairing mode under wifi debugging

On PC (Linux or Windows)

- `cd <android platform tools>`
- `./adb pair <ip:port>` this is provided by pairing screen on watch (if "unknown host service", run `adb kill-server` and try again)
- `./adb connect <ip:port>` the port is different from the pairing port and is provided on the wifi debug screen
- `./adb devices` to confirm connection

In WatchFaceStudio:

- Click Publish
- Set password to password
- May need to create keystore
- Build apk then use below adb method to install. Studio doesn't succeed pairing to watch in latest version 24/11/2023)

## To install built .apk (both phone and watch use adb connect):

```
./adb install -r <\path\to\app.apk>
```

If more than one device connected when installing,

- run: `./adb devices` then use device after -s:
- `./adb -s 192.168.6.88:5555 install -r <\path\to\app.apk>`

If error about signatures not matching existing package, uninstall previous installation:

- `./adb -s 192.168.6.88:5555 uninstall "com.watchfacestudio.led"`

## For Android phone: connecting phone to PC (unrelated to Watch)

Can use MTP mode for transfer (not necessary but maybe stops charging if not required).

If connect to phone does not work:

- Connect phone with usb cable to PC.
- `./adb usb`
- `./adb tcpip 5555`
- `./adb connect <phone ip address>`

If phone is connected via usb, you can use the USB device name eg something like RF8N10T8WCJ as the parameter to -s without using the `adb connect` command.

Useful links:

- https://forum.xda-developers.com/t/debloat-galaxy-watch-4.4324147/
- https://forum.xda-developers.com/t/watch4-adb-commands-disable-enable-uninstall-restore-system-app-install-pull-apps.4324063/
- https://developer.android.com/training/wearables/get-started/debugging - enable wifi debugging on watch

To find app ids, from PC:

```
adb connect 192.168.xxx.xxx:5555 (your watch IP address & enable debugging)
adb shell
pm list packages
```
