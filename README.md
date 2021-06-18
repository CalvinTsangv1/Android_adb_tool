# Android adb tools v1.0.31
adb tools is a toolkit included in the Android SDK package, adb which means [Android Debug Bridge](https://developer.android.com/studio/command-line/adb.html) that lets you communicate with a device.

**Note:** Some commands may depending on the version of Android system or ROM.

* [Basic Usage](#basic-usage)
    * [Command syntax](#command-syntax)
    * [Targeting equipment for command](#targeting-equipment-for-command)
    * [Start/Stop](#startstop)
    * [View adb version](#view-adb-version)
    * [Run adbd as root](#run-adbd-as-root)
    * [Designated adb server network port](#designated-adb-server-network-port)
	
* [Device connection management](#device-connection-management)
    * [Inquiries connected device / simulator](#inquiries-connected-device--simulator)
    * [USB connection](#usb-connection)
	
	
## Basic Usage

### Command syntax

adb basic syntax of the command is as follows:

```sh
adb [-d|-e|-s <serialNumber>] <command>
```

If only one device / emulator connection, you can omit the `[-d | -e | -s <serialNumber>]` this part, the direct use `adb <command>`.

### Targeting equipment for command

If you have more than one device / emulator connection, you need to specify the target device for the command.

| Parameter           | Meaning                                                                           |
|---------------------|-----------------------------------------------------------------------------------|
| -d                  | Specifies currently the only Android device USB connector as the command target   |
| -e                  | Specify currently the only goal for the command to run the simulator              |
| `-s <SerialNumber>` | Specifies the device number corresponding serialNumber / simulator command target |

In the case of multiple devices / simulators are connected to the more common `-s <serialNumber>` parameters, serialNumber can be obtained through `adb devices` command. Such as:

```sh
$ adb devices

List of devices attached
cf264b8f	device
emulator-5554	device
```

Output in the `cf264b8f` and` emulator-5554` is serialNumber. For example, this time I want to specify `cf264b8f` this equipment to run the adb command to take a screen resolution:

```sh
adb -s cf264b8f shell wm size
```

Encountered multiple devices / simulators use the case of these parameters for the command to specify the target device, hereinafter to simplify the description will not be repeated.

### Start/Stop

Start adb server command:

```sh
adb start-server
```

(Generally no need to manually execute this command, when you run the command adb adb server if found does not start automatically from the transfer.)

Stop adb server command:

```sh
adb kill-server
```

### View adb version

command:

```sh
adb version
```

Sample output:

```sh
Android Debug Bridge version 1.0.36
Revision 8f855a3d9b35-android
```

### Run adbd as root

The operating principle is adb adb server daemon and the phone side PC side adbd establish a connection, then the PC side adb client via adb server forward command parsing after running adbd receive commands.

So if adbd ordinary rights to perform some require root privileges to execute the command can not be directly used `adb xxx` execution. Then you can then execute the command `adb shell` after` su`, but also allows adbd root privileges to perform this high privilege can execute arbitrary commands.

command:

```sh
adb root
```

Normal output:

```sh
restarting adbd as root
```

Now run `adb shell`, take a look at the command line prompt is not turned into a` `#?

After some phone root can not let adbd by `adb root` execute commands with root privileges, some models such as Samsung, will be prompted to` adbd can not run as root in production builds`, then you can install adbd Insecure, then `adb root` try.

Accordingly, if you want to restore adbd non-root privileges, you can use `adb unroot` command.

### Designated adb server network port

command:

```sh
adb -P <port> start-server
```

The default port is 5037.


## Device connection management

### Inquiries connected device / simulator

command:

```sh
adb devices
```

Example output:

```sh
List of devices attached
cf264b8f	device
emulator-5554	device
```

Output format is `[serialNumber] [state]`, serialNumber that is, we often say that the SN, state the following categories:

* `Offline` - indicates that the device is not connected to the success or unresponsive.

* `Device` - device is connected. Note that this state does not identify the Android system has been fully activated and operational in the device during startup device instance can be connected to the adb, but after boot the system before it becomes operational.

* `No device` - no device / emulator connection.

The output shows the current connected the two devices / simulators, `cf264b8f` and` emulator-5554` are they SN. As can be seen from the `emulator-5554` name it is an Android emulator.

Common alarm output:

1. No device / emulator connection is successful.

   ```sh
   List of devices attached
   ```

2. The device / emulator is not connected to adb or unresponsive.

   ```sh
   List of devices attached
   cf264b8f	offline
   ```

### USB connection

USB connection normal use adb by the need to ensure that:

1. Hardware status is normal.

   Including Android devices in the normal power state, USB cable and interface intact.

Developer Options 2. Android devices and USB debugging mode is on.

   You can go to the "Settings" - "Developer options" - "Android Debug" view.

   If you can not find the developer options in the settings, it needs to make it through an egg is displayed: In the "Settings" - "About phone" continuous click "version number" 7 times.

3. The device driver is normal.

   It seems to worry about the Linux and Mac OS X, the Windows likely to be encountered in the case of the need to install drivers, this can be confirmed right "Computer" - "Properties", the "Device Manager" in view on related equipment Is there a yellow exclamation point or question mark, if not explain the driving state has been good. Otherwise, you can download a mobile assistant class program to install the driver first.

4. Status after confirmation via USB cable connected computers and devices.

   ```sh
   adb devices
   ```

   If you can see

   ```sh
   xxxxxx device
   ```

   Description Connection successful.
