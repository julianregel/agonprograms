# Agon Light Experimental Screen Modes

The current Agon Light VDP 1.03 release supports the following screen modes:

| Mode | Details | FabGL Mode |
|---|---|---|
| 0 | 1024x768 in 2 colours at 60Hz | SVGA_1024x768_60Hz |
| 1 | 512x384 in 16 colours at 60Hz | VGA_512x384_60Hz |
| 2 | 320x200 in 64 colours at 75Hz | VGA_320x200_75Hz |
| 3 | 640x480 in 16 colours at 60Hz | VGA_640x480_60Hz |

Some of these modes are problematic for a number of users and do not display correctly.

I have been experimenting with the VDP code and have created a build that incorporates a lot of new modes for testing. The following modes are suggested and should be compatible with all VGA monitors. In the testing I've done, if the monitor is running at 60Hz, then these modes appear to be scaled to 640x480. As the 640x240 and 320x240 resolutions can do this using pixel and/or line doubling, the picture is very crisp and clear.

## VGA standard resolutions and refresh rates

| Mode | Details | FabGL Mode | Notes |
|---|---|---|---|
| 0 | 640x480 in 16 colours at 60Hz | VGA_640x480_60Hz | VDP 1.03 Mode 3, VGA Mode 12h |
| 1 | 640x480 in 4 colours at 60Hz | VGA_640x480_60Hz |
| 2 | 640x480 in 2 colours at 60Hz | VGA_640x480_60Hz |
| 3 | 640x240 in 64 colours at 60Hz | VGA_640x240_60Hz |
| 4 | 640x240 in 16 colours at 60Hz | VGA_640x240_60Hz |
| 5 | 640x240 in 4 colours at 60Hz | VGA_640x240_60Hz |
| 6 | 640x240 in 2 colours at 60Hz | VGA_640x240_60Hz |
| 7 | Reserved for Teletext mode currently under development | | 
| 8 | 320x240 in 64 colours at 60Hz | QVGA_320x240_60Hz | VGA "Mode X" |
| 9 | 320x240 in 16 colours at 60Hz | QVGA_320x240_60Hz |
| 10 | 320x240 in 4 colours at 60Hz | QVGA_320x240_60Hz |
| 11 | 320x240 in 2 colours at 60Hz | QVGA_320x240_60Hz |
| 12 | 320x200 in 64 colours at 70Hz | VGA_320x200_70Hz | VGA Mode 13h |
| 13 | 320x200 in 16 colours at 70Hz | VGA_320x200_70Hz |
| 14 | 320x200 in 4 colours at 70Hz | VGA_320x200_70Hz |
| 15 | 320x200 in 2 colours at 70Hz | VGA_320x200_70Hz |

I did not include a 640x480 in 64 colours at 60Hz mode as this will require 300K for the frame buffer and I assume there isn’t sufficient memory in the ESP32 for this.

## SVGA resolutions and refresh rates

Additionally, beyond VGA, I have had success with the following:

| Mode | Details | FabGL Mode | Notes |
|---|---|---|---|
| 16 | 800x600 in 4 colours at 60Hz | SVGA_800x600_60Hz |
| 17 | 800x600 in 2 colours at 60Hz | SVGA_800x600_60Hz |
| 18 | 1024x768 in 2 colours at 60Hz | SVGA_1024x768_60Hz | VDP 1.03 Mode 0 |


## Additional modes requested for testing

The following modes are added for further testing, even if they fall outside of the original VGA mode specifications.

The 512x384 modes are quarter of 1024x768, so should display okay on most SVGA monitors. They are also pixel- and line-doubled 256x192 which was a common video mode found in many other 8bit computers including the ZX Spectrum and MSX computers. The 16 colour version of this is the current VDP 1.03 Mode 1. There does not appear to be sufficient RAM in the ESP32 to support this mode with 64 colours.

The 320x200 in 64 colours at 75Hz is the current VDP 1.03 Mode 2. This is reported to cause problems for some users and is likely due to the refresh rate.

The 640x512 resolution was requested as it would be a scaled down mode that should be suitable for SVGA monitors capable of 1280x1024. I cannot get this to display correctly on my monitor, but YMMV.

| Mode | Details | FabGL Mode | Notes |
|---|---|---|---|
| 19 | 512x384 in 16 colours at 60Hz | VGA_512x384_60Hz | VDP 1.03 Mode 1 |
| 20 | 512x384 in 4 colours at 60Hz | VGA_512x384_60Hz |
| 21 | 512x384 in 2 colours at 60Hz | VGA_512x384_60Hz |
| 22 | 320x200 in 64 colours at 75Hz | VGA_320x200_75Hz |  VDP 1.03 Mode 2 |
| 23 | 640x512 in 16 colours at 60Hz | QSVGA_640x512_60Hz | 


# Legacy Modes Support

In order to maintain compatability with programs that are designed for the modes in VDP 1.03, I have added the following:

## VDU 23,0,193,n

This VDU command will set the VDP to support the experimental modes listed above, or the original four modes defined in VDP 1.03. The options are:

- VDU 23,0,193,0 = Use "new" screen mode definitions (the default)
- VDU 23,0,193,1 = Use legacy screen modes as defined in VDP 1.03

This additional functionality requires updating both the video.ino and agon.h files.

Please note, running the MODE command after running the VDU command will only take effect if you are changing to a different screen mode. For example, if you are in mode 0, make the VDU change and then try and set mode 0 again, it will not recognise the new mode. You must change to a different mode first.
