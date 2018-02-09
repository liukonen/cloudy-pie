# cloudy-Pie (a remake of DJAkbar's cloudy-a project)
A  script for light animations for two LED strips according to the weather forecast. For an example see [Youtube](https://www.youtube.com/watch?v=DNXssI4LuMc)  

<img src="https://github.com/liukonen/cloudy-pie/blob/master/20180127_183138.jpg" width="300" height="400" />
![Cloud]())

## Instructions for use: 
Here is a short write-up of some instructions to make your own DIY weather forecast cloud lamp.  
The creator of this project built his using this with a Raspberry Pi 3. I am currently using a Rasberry Pi B.Some things should be set-up already:  
+ Python and pigpio installed on the Raspberry Pi  
+ An internet connection (if you’re not using WIFI make sure to have a cable that’s long enough)  
+ A WeatherUnderground API key and your location to append the link for the request. For more information see [here](https://github.com/InitialState/wunderground-sensehat/wiki/Part-1.-How-to-Use-the-Wunderground-API)
	+ Optional: the Apache Server for the future project of having a local webpage / maintenance 

## So what do you need:  
+ Raspberry Pi 
+ a SD card for your version of the PI, with Rasbian lite (or rasbian if you want to remote desktop into it later)
	+ I personally recomend the Lite version... faster updates, and less things running
+ Internet connectivity to your pi. Wifi setup directions can be found [here](https://thepihut.com/blogs/raspberry-pi-tutorials/83502916-how-to-setup-wifi-on-raspbian-jessie-lite). Cat cable connections are automatic.
+ RGB LED-strip with 3 Pins for RGB and 1 Strip for power 
	+ (I have used a length of 3m but shorter is also fine depending of how much of the strip you want to use) (I used the following [strips](https://www.amazon.com/gp/product/B073NXF8B7/ref=oh_aui_detailpage_o06_s00?ie=UTF8&psc=1), and split the power line between 2 of these off a single USB port. I used the MOSFETS below as regulators,  
+ A breadboard or perfboard 
	+(for the second option you’ll of course need a soldering iron too, I recommend this since it’s a bit more permanent and also saves space)
+ Some jumper wires (male to male, male to female)  
+ Six MOSFETs (IRLZ34N)  
+ A power jack and / or USB Power Supply  
- optional - Multiple socket strip (with at least two sockets, I used one with three sockets because they are more common). I have chosen a lenght of 5 meters, just keep in mind it has to run up to the ceiling, along the ceiling and then to the power outlet. (depends on how you are getting power to the device.  
For the lamp itself I recommend the following:  
+ A 5 liter (or 1 gallon) plastic bottle 
	+ (or a similarly big container of see-through material. Some people are using these IKEA paper lanterns but I do not recommend them since they could theoretically burn)  
+ Cotton batting. (found at my local craft store)  
+ A "cool" glue gun or similar to attach the batting to the bottle  

### If Making a true "Lamp" like I did
+ A light socket to power adapter

### If Hanging
+ Some fishing line (clear string might work too but keep in mind that it will be a bit heavy in the end)  
+ A hook for your ceiling  

## Ok, so what do you need to do? This:  
+ Set up two circuits according to this [Tutorial](http://popoklopsi.github.io/RaspberryPi-LedStrip/#!/)
+ To tweak the above tutorial for two strips you should keep two things in mind: first you will need a common ground for all the MOSFETs. But the 12 V should run directly from the end of the first strip to the beginning of the second. I have used jumper cables soldered to the strips and the perfboard, but any kind of cable should work.  
+ Ideally, the GPIO-pins should be connected as follows (one half follows the above tutorial; I will call them Strip A and Strip B here):  
+ Red A: GPIO 17, Blue A: GPIO 22, Green A: 24  
+ Red B: GPIO 26, Blue B: GPIO 12, Green B: 13
	+ (Original Pi B 24 pin layout, I went with Red B: GPIO 27, Blue B: GPIO 23, Green B GPIO 25
	+ If using the Model B, you will have to update the source code with the new pin numbers as well
	+ Great site for pin locations [here](https://www.raspberrypi-spy.co.uk/2012/06/simple-guide-to-the-rpi-gpio-header-and-pins/)
+ Now is a good moment to plug in the Raspberry Pi and its power supply. Use the multiple socket strip because they should run on the same power cord (since mine is on USB... they are on different USB ports). Keep the cables as short as possible. I just rolled them up and bound them together. Some people say you should not do this, I guess with such low Voltages there is no risk but be aware that I can not be held responsible if anything goes wrong with your electronics)  
+ To test the LEDs you should open a terminal and type the following “sudo pigpiod”, press enter and then “pigs p 26 255 p 13 255 p 12 255 p 17 255 p 22 255 p 24 255” (or adjust for the pins listed above if programing on the old model). When you press enter again, both LED strips should be completely white. If it doesn’t work please check if you connected everything correctly. You can try out other colours by changin the value 255 to something else. Experiment a bit, expecially how to get a good yellow. I suspect that the correct values for yellow change from strip to strip.  
+ Now we’ll have a look at the code. In the beginning you will have to insert the API key you got earlier from WeatherUndergound and the location for appending the link of the request. You can also decide at which time the lamp should display the forecast for the next day by changing the number of the variable switch_time (if you always want to display the weather for the moment just set it to 24).
+ If you fill in all the variables correctly we are already done here. To see if you have the correct data for it, insert it in the according places here and check this [link](http://api.wunderground.com/api/ API-KEY /forecast/q/ LOCATION .json)  
+ I recommend trying this first because if something is not working, the problem lies probably in the weather forecast request.  
+ It’s all working and you can see a text forecast for your location? Good, then we will install the code on the Pi now. For this you just have to copy it into the home/pi (or whatever is your username) folder of your Pi and safe it as cloudy-a.py (do this by the method you prefer, either by ssh or directly with a USB stick for example) and run “sudo chmod +x cloudy-a.py” while being in that folder.  
+ Now is probably a good time to see if we can get some lights, eh? Run the code with “sudo pigpiod” followed by “python cloudy-a.py”. When you have been sufficiently blinded by the LEDs, press CTRL-C to stop.  
+ If you are happy with starting the script every time manually, you can skip the next few steps and start building the actual lamp.But who wants to do that, right? So on we go:  
+ Create a new file in the home/pi (again: or whatever is your username) directory: navigate to the folder in the terminal and type “sudo nano launcher.sh” You will have created a new file where you should write the following:  
    #!/bin/sh  
    launcher.sh  
	    cd /  
	    cd home/pi  
	    sudo python cloudy-a.py  
	    cd /  
+ We now have to set up rules that this file is executed on every startup. For that you type in the terminal “sudo crontab -e”. At the end of the file insert the following two lines:  
	    @reboot /usr/bin/pigpiod  
	    @reboot /home/pi/launcher.sh    
+ Close the file with CTRL-X and restart with “sudo reboot”. Now the LED strips should light up according to the weather forecast already. We are finally ready to build the lamp!  

Just some notes: When you finished the lamp, put the Raspberry Pi, the breadboard or perfboard and the LED-strips all in and have everything attached to the socket strip adapter.  I used a rectangular 1 gallon watter bottle. I cut the bottom open for easy access, and kept the cutout as a lid I could tape shut. I also added some air holes on the top for ventalation and kept the opening of the jug open for even more air flow. On the bottom I cut a hole perfect for the lamp to hold the jug, and since the lamp had a threaded piece to it, I was able to secure it down.

The designer bought a cheap timeclock because they didn’t want the lamp to be switched on all the time, but obviusly this is up to you. The program is set up so that it will finish the cycle of animation ~~about~~ every hour (and can be adjusted in code). It will then check for the forecast again and start the according animation. 

Anyway, now everything should be finished and you have a working weather forecast cloud lamp. I want to stress again that I can not be held responsible for any damages of injuries that might occur in the making or running of this project. Of course I can also not guarantee that it will work for you even if you follow all the steps correctly. If you have suggestions I would be happy to hear them. I will try to answer questions but don’t count on it. Now I would be very happy to see your creations!  

## Optional Steps
This device is a mini computer running in a plastic tub, and as such, runs into the same problems with any computer. Here are some maintenance and options to do after the fact
+ [Install WebMin](http://www.instructables.com/id/Adding-Webmin-to-manage-a-Raspberry-Pi/)
	+ this gives you the abilty to maintain, update, run cron jobs, etc
+ If running the full version of Rasbian, [Remote Desktop access](https://www.raspberrypi.org/magpi/vnc-raspberry-pi/)
+ Run my steps on installing the CloudyPie landing page (coming soon), giving you access to any errors, and forcast information your pie pulled down (and in future verisons, control of your lamp)
+ Have your pi auto update. Directions [here](https://blog.dantup.com/2016/04/setting-up-automatic-updates-on-raspberry-pi-raspbian-jessie/)

Photos of my project can be found [here!](https://photos.app.goo.gl/HpOL2VZhOtbpGhuh1)
