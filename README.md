# Pygame Runner for Retro Portables

This project provides a python/pygame environment for portable retro game 
consoles running EmuElec-based Linux operating systems. It has been developed 
for and tested on an RG351p running 351elec, but should run on other devices 
using similar architectures.

Retro game consoles do not provide a terminal so no interactive environment is 
provided. Instead, a run_game binary is used to launch a python module. The 
command line program expects two parameters, the working directory and the 
module to be loaded. Any additional parameters are passed to the program.

## Contents
- Porting Pygame Projects
- Example Script
- Built in Games
- Building from Source


# Porting Pygame Projects

Porting pygame projects can be tricky. The following suggestions should be 
adhered to:

1. The game should scale to 480x320 for RG351 devices. The SCALED flag 
for set_mode() or Window() should be used to properly upscale to higher 
resolution devices.

2. The game should use the new gamecontroller API provided by recent versions 
of pygame, and startup script should set the SDL_GAMECONTROLLERCONFIG 
environment variable to ensure that the buttons and other controls are properly 
configured. Alternatively, [gptokey 
](https://github.com/EmuELEC/gptokeyb/blob/main/README.md) may be used for 
keyboard emulation.

3. For best performance, use the new GPU rendering features provided by 
pygame.sdl2.video.

# Example script

Here is an example script that will launch Cooties using the gamecontroller API.

```
#!/bin/bash

if [ -d "/opt/system/Tools/PortMaster/" ]; then
  controlfolder="/opt/system/Tools/PortMaster"
elif [ -d "/opt/tools/PortMaster/" ]; then
  controlfolder="/opt/tools/PortMaster"
else
  controlfolder="/roms/ports/PortMaster"
fi

source $controlfolder/control.txt
export TEXTINPUTINTERACTIVE="Y"
echo game script
get_controls

GAMEDIR=/$directory/ports/pygame

cd $GAMEDIR

$ESUDO chmod 666 /dev/uinput
$GPTOKEYB "run_game" $HOTKEY textinput -c "./run_game.gtpk" &
SDL_GAMECONTROLLERCONFIG="$sdl_controllerconfig" ./run_game Cooties run_game 
console | tee -a ./log.txt
$ESUDO kill -9 $(pidof gptokeyb)
$ESUDO systemctl restart oga_events &
printf "\033c" >> /dev/tty1
```

# Built in Games:
- [Aeroblaster](https://dafluffypotato.itch.io/aeroblaster), by DaFluffyPotato
- [Cooties](https://mcpalmer1980.itch.io/cooties), by mcpalmer1980 
- [Flyboy](https://mcpalmer1980.itch.io/flyboy), by mcpalmer1980
- [Hoverball](https://github.com/mcpalmer1980/pygame-hoverball), by 
mcplamer1980
- [Spike Dungeon](
https://dafluffypotato.itch.io/spike-dungeon), by DaFluffyPotato
- [Squirrel Eat Squirrel](http://inventwithpython.com/pygame/), by Al Sweigart
- [Star Pusher](http://inventwithpython.com/pygame/), by by Al Sweigart
- [Which Way is Up?](https://www.pygame.org/project/410), by Olli Etuaho

# Building from Source
run_pygame should be build using an aarch64 chroot, such as that provided by 
Christian Haitian's [virtualbox 
image](https://drive.google.com/file/d/1_6SLtNurqeUafKrbBM2Ba0fTWyZkGAGi/view).

```
apt install build-dep libsdl2 libsdl2-image libsdl2-mixer libsdl2-ttf 
libfreetype6 python3 libportmidi0

apt install libjpeg-dev libwebp-dev libtiff5-dev libsdl2-image-dev 
libsdl2-image-2.0-0 -y

apt install libjpeg-dev libtiff5-dev libsdl2-image-dev libsdl2-image-2.0-0 -y

apt install git python-dev python-numpy python-opengl libsdl-image1.2-dev 
libsdl-mixer1.2-dev libsdl-ttf2.0-dev libsmpeg-dev libsdl1.2-dev 
libportmidi-dev 
libswscale-dev libavformat-dev libavcodec-dev libtiff5-dev libx11-6 
libx11-dev fluid-soundfont-gm timgm6mb-soundfont xfonts-base xfonts-100dpi 
xfonts-75dpi xfonts-cyrillic fontconfig fonts-freefont-ttf libfreetype6-dev

apt install libporttime-dev
apt install libportmidi-dev

git clone https://github.com/pygame/pygame.git
cd pygame

python3 setup.py -config -auto -sdl2
python3 setup.py install --user

pip3 install pyinstaller
pip3 install numpy

git clone https://github.com/mcpalmer1980/pygame_run.git
cd pygame_run
pyinstaller pygame_run.spec
```

Pyinstaller will generate its binary in a dist subfolder and add many other 
dependencies to it. It might not collect all of the necessary files, however. 
You may need to add the missing files to the pygame and numpy folders that 
pyinstaller generates.
