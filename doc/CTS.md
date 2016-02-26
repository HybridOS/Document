##Compatibility Test Suite (CTS)

The CTS is a free, commercial-grade test suite, available for download. The CTS represents the "mechanism" of compatibility.The CTS runs on a desktop machine and executes test cases directly on attached devices or an emulator. The CTS is a set of unit tests designed to be integrated into the daily workflow (such as via a continuous build system) of the engineers building a device. Its intent is to reveal incompatibilities early on, and ensure that the software remains compatible throughout the development process.

##Setting up CTS

###1.Desktop machine setup

**ADB and AAPT**

Before running the CTS, make sure you have recent versions of both Android Debug Bridge (adb) and Android Asset Packaging Tool (AAPT) installed and those tools' location added to the system path of your machine.

To install ADB, download the Android SDK Tools package for your operating system, open it, and follow the instructions in the included README file. 

Ensure adb and aapt are in your system path. The following command assumes you've opened the package archive in your home directory:

export PATH=$PATH:$HOME/android-sdk-linux/platform-tools

Note: Please ensure your starting path and directory name are correct.

**Java Development Kit (JDK)**

You need to install the proper version of the Java Development Kit (JDK):

- CTS 5.0 and later: Java 7

- CTS 4.4 and earlier: Java 6 

**CTS files**

Download and open the CTS packages matching your devices' Android version and all the Application Binary Interfaces (ABIs) your devices support.

Download and open the latest version of the CTS Media Files.

###2.Android device configuration

1.Factory data reset the device: Settings > Backup & reset > Factory data reset

Warning: This will erase all user data from the device.

2.Set your device’s language to English (United States) from: Settings > Language & input > Language

3.Turn on the location setting if there is a GPS or Wi-Fi / Cellular network feature on the device: Settings > Location

4.Connect to a Wi-Fi network that supports IPv6, can treat the Device Under Test (DUT) as an isolated client (see the Physical Environment section above), and has an internet connection: Settings > Wi-Fi

5.Make sure no lock pattern or password is set on the device: Settings > Security > Screen lock = 'None'

6.Enable USB debugging on your device: Settings > Developer options > USB debugging.

Note: On Android 4.2 and later, Developer options is hidden by default. To make them available, go to Settings > About phone and tap Build number seven times. Return to the previous screen to find Developer options. See Enabling On-device Developer Options for additional details.

7.Select: Settings > Developer options > Stay Awake

8.Select: Settings > Developer options > Allow mock locations

Note: Starting in Android 6.0, this mock locations step is neither available nor required.

9.Launch the browser and dismiss any startup/setup screen.

10.Connect the desktop machine that will be used to test the device with a USB cable

Note: When you connect a device running Android 4.2.2 or later to your computer, the system shows a dialog asking whether to accept an RSA key that allows debugging through this computer. Select Allow USB debugging.
Install and configure helper apps on the device.

Note: For CTS versions 2.1 R2 through 4.2 R4, set up your device (or emulator) to run the accessibility tests with:
adb install -r android-cts/repository/testcases/CtsDelegatingAccessibilityService.apk
On the device, enable: Settings > Accessibility > Accessibility > Delegating Accessibility Service

Note: For CTS 2.3 R4 and beyond on devices that declare the android.software.device_admin feature, set up your device to run the device administration tests with:

adb install -r android-cts/repository/testcases/CtsDeviceAdmin.apk

On the device, enable only the two android.deviceadmin.cts.CtsDeviceAdminReceiver* device administrators under: Settings > Security > Select device administrators. Make sure the android.deviceadmin.cts.CtsDeviceAdminDeactivatedReceiver and any other preloaded device administrators stay disabled in the same menu.

##connect to android-x86(adb connect using wifi)
（1）start adbd service

open the terminal and type these commands:
~~~
    su
    setprop service.adb.tcp.port 5555
    stop adbd
    start adbd
~~~ 
check your device ip (eg.192.168.1.193)

（2）adb connect using your machine (eg.ubuntu）

type：adb connect 192.168.1.193 （the ip of android-x86）

warning：using the same  LAN

If successed，it can show already connected to 192.168.1.193:5555

（3）test if the connection is successful
type：adb shell 

If you can enter the shell of android-x86,it show you have been conneted to the test machine.


##Running CTS tests

###1.Using the CTS tradefed

1.Connect at least one device.

2.Press the home button to set the device to the home screen at the start of CTS.

3.While a device is running tests, it must not be used for any other tasks and must be kept in a stationary position (to avoid triggering sensor activity) with the cameras pointing at an object that could be focused.

4.Do not press any keys on the device while the CTS is running. Pressing keys or touching the screen of a test device will interfere with the running tests and may lead to test failures.

5.Launch the CTS console by running the cts-tradefed script from the folder where the CTS package has been unzipped, e.g. $ ./android-cts/tools/cts-tradefed

6.You may start the default test plan (containing all of the test packages) by appending: run cts --plan CTS This will kick off all the CTS tests required for compatibility. Enter list plans to see a list of test plans in the repository. Enter list packages to see a list of test packages in the repository. See the CTS command reference or type help for a complete list of supported commands.

7.Alternately, you may run the CTS plan of your choosing from the command line using: cts-tradefed run cts --plan

8.View test progress and results reported on the console. 

###2.Selecting CTS plans

The following test plans are available:

 - CTS—all tests required for compatibility.
 - Signature—the signature verification of all public APIs
 - Android—tests for the Android APIs
 - Java—tests for the Java core library
 - VM—tests for ART or Dalvik
 - Performance—performance tests for your implementation

These can be executed with the run cts command.