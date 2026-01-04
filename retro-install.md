# Installing everything needed for Z80-Retro! development

Other than for the task of programming the boot FLASH with a Raspberry PI, the following should work on most Linux systems.
You might have to change the `apt` commands to use your system's package manager if you are not using a Debian style system.

The following commands have been verified on a Raspberry PI configured as described here: https://github.com/johnwinans/raspberry-pi-install

```
sudo apt install build-essential libi2c-dev z80asm cpmtools
sudo apt install kicad
sudo apt install minicom

GIT_REPOS=http://github.com/Z80-Retro  # Use this for anonymous downloading (if you don't have a github account)
#GIT_REPOS=git@github.com:Z80-Retro    # Uncomment & change this if you use ssh and/or have your own forks

mkdir ~/retro
cd ~/retro
git clone --recurse-submodules $GIT_REPOS/2063-Z80-cpm.git
git clone $GIT_REPOS/2065-Z80-programmer.git
git clone $GIT_REPOS/example-filesystem.git
git clone $GIT_REPOS/pcb-standoffs
git clone $GIT_REPOS/2068-Z80-TMS9118

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

