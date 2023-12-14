# SIOT-Coursework
I investigated the link between pipe wall thickness and acoustic signal monitoring within a cast iron waster network to link certain signal features to inferable information about the remaining lifetime of cast iron pipes. My product was a high-frequency sampling microphone that returned a list of the top most powerful frequencies measured in a sample.
# parts list
- Raspberry Pi Pico 2020
- NodeMCU V3(esp8266 wifi module embedded)
- mic audio amplifier module
- Raspberry pi or Linux system(optional)
- GL-net mini smart wifi router(optional)
- 2 micro sub cables
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
I set up the MQTT broker following this tutorial:
I would advise it if it's your first time using AWS. The only thing is that I removed the secret file from the Arduino sketch and copied the key assignments directly into the main script. This was just because I found that it produced errors when uploading it to my NodeMCU.
