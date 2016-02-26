# Building the System

The following instructions to build the **LOLLIPOP-X86 SOURCE CODE**. The basic sequence of build commands is as follows:

## Set up environment

nitialize the environment with the envsetup.sh script. Note that replacing source with . (a single dot) saves a few characters, and the short form is more commonly used in documentation.

 ```
source build/envsetup.sh
 ```

or

 ```
. build/envsetup.sh
 ```

## Choose a Target

Choose which target to build with lunch. The exact configuration can be passed as an argument. First you should run the following command:

 ```
lunch
 ```

Then you will see many targets. In our project, we usually choose "android_x86_64-x" targets to build. There are three kinds of build type:

BuildType | Use |
--------- | --- |
user      | limited access; suited for production |
userdebug | like "user" but with root access and debuggability; preferred for debugging |
eng       | development configuration with additional debugging tools |

For more information about building for and running on actual hardware, see [Running Builds](https://source.android.com/source/running.html).

## Build the code

Build everything with make. GNU make can handle parallel tasks with a -jN argument, and it's common to use a number of tasks N that's between 1 and 2 times the number of hardware threads on the computer being used for the build. For example, on a dual-E5520 machine (2 CPUs, 4 cores per CPU, 2 threads per core), the fastest builds are made with commands between make -j16 and make -j32.

And for openthos, we want to run our system oe PC. So you should build it into an image . You can add a "iso_img" to build a .iso image and it can boot into lengacy BIOS-compatibility mode:

 ```
make -jX iso_img
 ```

To boot into UEFI mode, you should build a efi_img instead:

 ```
make -jX efi_img
 ```

Waiting for the build complete and run your own image!
