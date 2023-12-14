# SIOT-Coursework
I investigated the link between pipe wall thickness and acoustic signal monitoring within a cast iron waster network to link certain signal features to inferable information about the remaining lifetime of cast iron pipes. My product was a high-frequency sampling microphone that returned a list of the top most powerful frequencies measured in a sample.
# parts list
- Raspberry Pi Pico 2020
- NodeMCU V3(esp8266 wifi module embedded)
- mic audio amplifier module
- Raspberry pi or Linux system(optional)
- GL-net mini smart wifi router(optional)
- 2 micro sub cables

# Wiring Layout:
![image](https://github.com/rorybateman/SIOT-Coursework/assets/70752417/94767a2a-88df-46bf-bc7a-9e47f521cc16)
# Custom installation tools
## CMAKe
To program the Raspberry Pi with c programming you must configure Cmake build tools on your system. I would advise that you do this on a Linux-based system as the installation is much simpler. If you would like you can also configure it with Visual Studio however I found it easier to make edits on my PC before pushing it to GitHub and pull that change onto my Pi(4GB) where I would run all the necessary cmake configurations locally before dropping the UF2 file onto the Pico. Raspberry Pi has released a really good resource for getting started I would recommend it to anyone like myself who hasn't used or interacted with this before.
Raspberry Pi resource: https://datasheets.raspberrypi.com/pico/getting-started-with-pico.pdf
## Arduino IDE 
To program and monitor the NOdemcu in the Arduino IDE you have to have a couple of libraries installed I used this tutorial to ensure I had all the correct build libraries: https://projecthub.arduino.cc/najad/using-arduino-ide-to-program-nodemcu-67d448
Additionally, you will need the ARduino_JSon library installed as well as the PubSubClient by Nick OLeary installed.
# The code:
## Picos code
The Picos code just samples an analog signal before it sends out its package through its serial port to the NodeMCU.
So if you could just drag and drop the UF2 file I have provided onto you Pico it should work fine however if you wish to adjust it it's a little more work. The code that runs off the Pico is heavily based on the code supplied in this tutorial by Alex Wulf: https://www.hackster.io/AlexWulff/adc-sampling-and-fft-on-raspberry-pi-pico-f883dd
That code is again an adaptation of what exists within the examples provided by the Raspberry Pi resource: https://datasheets.raspberrypi.com/pico/getting-started-with-pico.pdf
If you don't fuss about it and just get something working I would recommend Alex Wulf's guide however it is very easy to make a mistake so I'd say its worth going through the Raspberry Pi resource as well. The changes I have made to the code have mostly been in the ADC.FFT file so if you want to make a quick change to the serial formatting do it in there, and then follow Alex Wulf's tutorial on how to update the UF2 file.
## NodeMCU's code:
The NodeMCU effectively reads the string supplied by the serial port before cutting this data packet up into its frequency components and sending it to AWS through an MQTT broker.
I set up the MQTT broker following this tutorial:https://how2electronics.com/connecting-esp8266-to-amazon-aws-iot-core-using-mqtt/
I would advise it if it's your first time using AWS. The only thing is that I removed the secret file from the Arduino sketch and copied the key assignments directly into the main script. This was just because I found that it produced errors when uploading it to my NodeMCU.
Because the serial read in as a string how I break down the code can get a little confusing so I have attached a key above to help explain.
# AWS
The AWS is configured with the following flow: 

![image](https://github.com/rorybateman/SIOT-Coursework/assets/70752417/88732408-3af7-4838-942c-83fa515b6c66)

I would advise following these tutorials if you want to recreate a system like this:

-MQTT to the time stream:https://www.youtube.com/watch?v=z8T4hAERuOg&t=629s
-Timstream to Grafana: [https://www.youtube.com/watch?v=z8T4hAERuOg&t=629s](https://www.youtube.com/watch?v=HuJycX1NEso)
Additional notes on this, when setting up Grafana you have to install the Timestream plugin, and to do this you have to set your workspace to allow default users admin privileges.
-SNS to SMS:https://www.youtube.com/watch?v=XRJBFWlOSJ8
This one is for email but you can easily alter it to mobile.

Here are some images of how I set up my Grafana page too as there the information online is fairly spread out.
![image](https://github.com/rorybateman/SIOT-Coursework/assets/70752417/5b8e33a9-c6af-4fff-9655-a039132243cb)
![image](https://github.com/rorybateman/SIOT-Coursework/assets/70752417/7559e1ba-0473-4553-a577-c55c7e5685da)

# Thankyou
Thanks for following along hopefully some of this information helps you with your own IOT endavours :).
