#!/usr/bin/env python3
import sys, os, importlib

print('starting pygame runner for 351elec\n  by Michael C Palmer')
module = sys.argv[2] if len(sys.argv) > 2 else None
folder = sys.argv[1] if len(sys.argv) > 1 else None
if module == None or folder == None:
	print('you must supply a folder and a startup script/module')
	print('   ex: ./run_game folder_name file_name')
	print('   loading test script')
	folder, module = 'test', 'main'

if 'SDL_GAMECONTROLLERCONFIG' in os.environ:
    print('SDL controller config found')
    print('  using: ' + os.environ['SDL_GAMECONTROLLERCONFIG'])
else:
    print('SDL controller config not found')
    if os.path.exists('/dev/input/by-path/platform-ff300000.usb-usb-0:1.2:1.0-event-joystick'):
        var = "03000000091200000031000011010000,OpenSimHardware OSH PB Controller,a:b1,b:b0,x:b3,y:b2,leftshoulder:b4,rightshoulder:b5,dpdown:h0.4,dpleft:h0.8,dpright:h0.2,dpup:h0.1,leftx:a0~,lefty:a1~,leftstick:b8,lefttrigger:b10,rightstick:b9,back:b7,start:b6,rightx:a2,righty:a3,righttrigger:b11,platform:Linux,"
    elif os.path.exists('/dev/input/by-path/platform-odroidgo2-joypad-event-joystick'):
        var = "190000004b4800000010000001010000,GO-Advance Gamepad (rev 1.1),a:b1,b:b0,x:b2,y:b3,leftshoulder:b4,rightshoulder:b5,dpdown:b9,dpleft:b10,dpright:b11,dpup:b8,leftx:a0,lefty:a1,back:b12,leftstick:b13,lefttrigger:b14,rightstick:b16,righttrigger:b15,start:b17,platform:Linux,"
    elif os.path.exists('/dev/input/by-path/platform-odroidgo3-joypad-event-joystick'):
        var = "190000004b4800000011000000010000,GO-Super Gamepad,platform:Linux,x:b2,a:b1,b:b0,y:b3,back:b12,guide:b14,start:b13,dpleft:b10,dpdown:b9,dpright:b11,dpup:b8,leftshoulder:b4,lefttrigger:b6,rightshoulder:b5,righttrigger:b7,leftstick:b15,rightstick:b16,leftx:a0,lefty:a1,rightx:a2,righty:a3,platform:Linux,"
    else:
        var = "19000000030000000300000002030000,gameforce_gamepad,leftstick:b14,rightx:a3,leftshoulder:b4,start:b9,lefty:a0,dpup:b10,righty:a2,a:b1,b:b0,guide:b16,dpdown:b11,rightshoulder:b5,righttrigger:b7,rightstick:b15,dpright:b13,x:b2,back:b8,leftx:a1,y:b3,dpleft:b12,lefttrigger:b6,platform:Linux,"          
    print('  setting controller variable:\n' + var)
    os.environ['SDL_GAMECONTROLLERCONFIG'] = var

sys.argv = sys.argv[2:] if len(sys.argv) > 2 else sys.argv
full_path = os.path.abspath(folder)
if os.path.isdir(full_path):
	sys.path.insert(0, full_path)
	os.chdir(full_path)
	if importlib.util.find_spec(module):
		print('launching {} module from {} folder'.format(module, full_path))
		importlib.import_module(module)
		print('script exited successfully')			
	else:
		print('cannot import {} from {} folder'.format(module, folder))
else:
	print('folder {} not found'.format(folder))

if False:
	import pygame, pymunk, numpy, json, cffi, sqlite3

