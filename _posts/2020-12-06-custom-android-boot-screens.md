---
layout: post
title: Custom android boot screens.
---

As part of a fun sunday evening project, I created a custom android boot screen. These animations are stored in a bootanimation.zip file. Android selects a boot animation zipfile from the following locations, in order:
'''

	/system/media/bootanimation-encrypted.zip (if getprop("vold.decrypt") = '1')
	/system/media/bootanimation.zip
	/oem/media/bootanimation.zip
'''
## [Creating a custom android boot animation](#custom-boot-animation)

## [warning](#warning)
Before starting the custom animation, it is a good idea to back-up your existing boot animation. For this you need adb and the developer options enabled.

'''

	#start an adb root shell. only the root user can access the /system partition
	adb root
	#remount the partions with the permissions of the root user
	adb remount
	#pull the file from the phone
	adb pull /system/media/bootanimation.zip
	#make a backup file for easy restore.
	mv bootanimation.zip bootanimation.zip.bak
'''


## [The layout of the zipfile](#zipfile-layout)
When you open the bootanimation.zip file, it includes:

'''
	desc.txt - a text file
	part0  \
	part1   \  directories full of PNG frames
	...     /
	partN  /

'''


## [desc.txt format](#desc-format)

The first line defines the general parameters of the animation:
'''
	WIDTH HEIGHT FPS
	
	    WIDTH: animation width (pixels)
	    HEIGHT: animation height (pixels)
	    FPS: frames per second, e.g. 60
'''
for example:
'''
	1080 1920 24
'''

The line above is followed by a number of rows of the form:
'''
	TYPE COUNT PAUSE PATH [FADE [#RGBHEX [CLOCK1 [CLOCK2]]]]
	
	    TYPE: a single char indicating what type of animation segment this is:
	        p -- this part will play unless interrupted by the end of the boot
	        c -- this part will play to completion, no matter what
	        f -- same as p but in addition the specified number of frames is being faded out while continue playing. Only the first interrupted f part is faded out, other subsequent f parts are skipped
	    COUNT: how many times to play the animation, or 0 to loop forever until boot is complete
	    PAUSE: number of FRAMES to delay after this part ends
	    PATH: directory in which to find the frames for this part (e.g. part0)
	    FADE: (ONLY FOR f TYPE) number of frames to fade out when interrupted where 0 means immediately which makes f ... 0 behave like p and doesn't count it as a fading part
	    RGBHEX: (OPTIONAL) a background color, specified as #RRGGBB
	    CLOCK1, CLOCK2: (OPTIONAL) the coordinates at which to draw the current time (for watches):
	        If only CLOCK1 is provided it is the y-coordinate of the clock and the x-coordinate defaults to c
	        If both CLOCK1 and CLOCK2 are provided then CLOCK1 is the x-coordinate and CLOCK2 is the y-coodinate
	        Values can be either a positive integer, a negative integer, or c
	            c -- will centre the text
	            n -- will position the text n pixels from the start; left edge for x-axis, bottom edge for y-axis
	            -n -- will position the text n pixels from the end; right edge for x-axis, top edge for y-axis
	            Examples:
	                -24 or c -24 will position the text 24 pixels from the top of the screen, centred horizontally
	                16 c will position the text 16 pixels from the left of the screen, centred vertically
	                -32 32 will position the text such that the bottom right corner is 32 pixels above and 32 pixels left of the edges of the screen
'''
	
For example:
'''
	p O 0 part0
'''

The first p will play the animation unless interupted by the boot
the first count is set to zero to loop until boot is complete
the second pause is set to 0, so no delay is added. To get this working correctly, I made sure to use a gif that can be looped.

For my simple animation, the folder part0 was sufficient, although you can assign multiple parts to your animation. you can use your own images for this animation in .png format. In my case the numbering for the images goes from 0001.png to nnnn.png

A helpful conversion and creation tool can be found [here](https://github.com/iamantony/create_android_bootanimation). it allows you to convert gifs or images to a bootanimation.zip file. 
 

## loading and playing frames

Each part folder is scanned and loaded directly from the zip archive. Within a part directory, every file ( is expected to be a PNG file that represents one frame in that part (at the specified resolution). For this reason it is important that frames be named sequentially (e.g. part000.png, part001.png, ...) and added to the zip archive in that order.

[source](https://android.googlesource.com/platform/frameworks/base/+/master/cmds/bootanimation/FORMAT.md)
