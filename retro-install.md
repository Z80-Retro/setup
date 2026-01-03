# Installing everything needed for Z80-Retro! development

Other than for the task of programming the boot FLASH with a Raspberry PI, the following should work on most Linux systems.
You might have to change the `apt` commands to use your system's package manager if you are not using a Debian style system.

The following commands have been verified on a Raspberry PI configured as described here: https://github.com/johnwinans/raspberry-pi-install

```
sudo apt install build-essential libi2c-dev z80asm cpmtools
sudo apt install kicad
sudo apt install minicom

mkdir ~/retro
cd ~/retro
git clone --recurse-submodules git@github.com:Z80-Retro/2063-Z80-cpm.git
git clone git@github.com:Z80-Retro/2065-Z80-programmer.git
git clone git@github.com:Z80-Retro/example-filesystem.git

cd ~/retro/2065-Z80-programmer/pi
make world

cd ~/retro/2063-Z80-cpm
cat - > Make.local <<EOF
SD_HOSTNAME=retro
SD_DEV=/dev/sdb1
EOF
make
cd boot
~/retro/2065-Z80-programmer/pi/flash < firmware.bin
cd ../filesystem
make burn
minicom -D /dev/ttyUSB0
```			

