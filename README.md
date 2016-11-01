# nexmon
Nexmon is our C-based firmware patching framework for Broadcom/Cypress WiFi chips 
that enables you to write your own firmware patches, for example, to enable monitor
mode with radiotap headers and frame injection.

Before we started to work on this repository, we developed patches for the Nexus 5 (with bcm4339 WiFi chip) in the [bcm-public](https://github.com/seemoo-lab/bcm-public)  repository and those for the Raspberry Pi 3 (with bcm43438 WiFi chip) in the [bcm-rpi3](https://github.com/seemoo-lab/bcm-rpi3) repository. To remove the development overhead of maintaining multiple separate repositories, we decided to merge them in this repository and add support for some additional devices. In contrast to the former repositories, here, you can only build the firmware patch without drivers and kernels. The Raspberry Pi 3 makes an exception, as here it is always required to also build the driver.

# Supported Devices
The following devices are currently supported by our nexmon firmware patch.

WiFi Chip | Firmware Version | Used in           | Operating System |  M  | RT  |  I  | FP  | UC  | CT 
--------- | ---------------- | ----------------- | ---------------- | --- | --- | --- | --- | --- | ---
bcm4330   | 5_90_100_41_sta  | Samsung Galaxy S2 | Cyanogenmod 13.0 |  X  |  X  |     |  X  |  X  |    
bcm4339   | 6_37_34_43       | Nexus 5           | Android 6 Stock  |  X  |  X  |  X  |  X  |  X  |  X 
bcm43438  | 7_45_41_26       | Raspberry Pi 3    | Raspbian 8       |  ?  |  ?  |  ?  |  ?  |  ?  |  ? 
bcm4358   | 7_112_200_17_sta | Nexus 6P          | Android 7 Stock  |  X  |  X  |     |  X  |  X  |  X 

## Legend
- M = Monitor Mode
- RT = Monitor Mode with RadioTap headers
- I = Frame Injection
- FP = Flash Patching
- UC = Ucode Compression
- CT = c't Article Support

# Structure of this repository
* `buildtools`: Contains compilers and other tools to build the firmware
* `firmwares`
  * `<chip version>`
    * `<firmware version>`
* `patches`
  * `<chip version>`
    * `<firmware version>`
      * `nexmon`
        * Makefile: Used to build the firmware
        * patch.ld: Linker file
        * src
          * patch.c: General patches to the firmware
          * injection.c: Code related to frame injection
          * monitormode.c: Code related to monitor mode with radiotap headers
          * ioctl.c: Handling of custom IOCTLs
        * obj (generated by Makefile): Object files created from C files
        * log (generated by Makefile): Logs written during compilation
        * gen (generated by Makefile): Files generated during the build process
    * common
      * wrapper.c: Wrappers for functions that already exist in the firmware
      * ucode_compression.c: [tinflate](http://achurch.org/tinflate.c) based ucode decompression
      * radiotap.c: RadioTap header parser
      * helper.c: Helpful utility functions
    * include
