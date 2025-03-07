# esp32-fingerprint-intercom
an ESP32 for interfacing a fingerprint reader and an intercom to put the data to Homeassistant


abstract
I use a nuki smart lock and a self build fingerprint reader circuit and controll them via an esp32 microcontroller and Homeassistant. The intercom system is integrated for using the buzzer and as an interrupt to wake the components (battery driven). The project uses esphome for connecting to my central Homeassistant instance. 
This here should be a short description of the project, I'm thinking about providing a github repo for files and so on.


Longer Story
Its about the interest and deployment of a Nuki Smart Lock Pro Gen4 that brought some more thinking into it. 
I'm an engineer and so I always thinker and play around with stuff like computers, raspberry pis, esp32s and so on. One day my interest grow on the side of smart locks. As I'm a huge fan and frequent deployer of Homeassistant in friends and familys homes, the smart lock pro from nuki seemed nice cause of it's bride-less capabilities to smart home stations like Google, Apple or: HomeAssistant (HA)! 
So, I ordered one. After recieving it and troubleshooting the mounting everything worked great with the Nuki App and with HA (via MQTT). But to always open the App (either way HA or Nuki) seems odd. You stand in front of your own door and need some moments to unlock it - even though I always have the key for the door in my pocket. First thing I tried was to do it via NFC. I had a NFC tag in the hallway that I could read with the back of my phone and so the door was unlocked (via a HA-Automation). Unfortunately Android phones do only read tags if they are unlocked (I think its a security feature) so I ended up unlocking my phone prior to reading the tag. It worked quite well but it was not convienient. 
Surely I'm aware of proximity based stuff like bluetooth and BLE but that could be a topic for later. I thought about the lock and nuki and saw their off the shelf fingerprint reader. Can esp-microcontrollers work with fingerprint readers? Shurely! There is a part called R503, its capacitive and cheap. I ordered one, soldered it to the esp and it worket flawless. 
A friend of mine was experimenting with door-access and fingerprint readers as well (off the shelf parts). I heard he had a box to wire up to his doorbell that he can use the intercom buzzer to open the main door (and speak to the person ringing but with a high latency). I was interested cause sometimes using of the buzzer could be useful. I studied how the intercom system in my flat works and found out its quite simple. It consists of an old Siedle 6+n system that is widely used in German homes. After digging some schematics and forums I found a nice discussion and many helpful ressources (in German).
After understanding how its done I wired up an esp32 and some components and the doorbell was in my HA - nice! I was getting notifications if somebody rang the doorbell and I could use the intercom buzzer via my phone. So I was eager to combine the stuff on one esp to simplify the electronics. It wasn't difficult! 
At that time I used a simply power supply to power it up but I had no outled close to my intercom. So I decided to power it by a battery. After some tinkering with battery boards for 18650 (I had good and realy bad boards) I found flat LiPo Cells with an integrated protection circuit. They fitted much more nicely to my esp and my stripboard and worked well. I figured out that an esp with an active wifi connection and an active fingerprint reader draws a realatively high current. Way to much for some months of battery life. So the task was to monitor the battery life and reduce the current drawn over time! Fortunately sleep modes for an esp32 are easy to use and now it goes to sleep and is waken up by an interrupt pin from my intercom. The fingerprint reader is also now sleeping if not in use. 
After all that tinkering and 1 or 2 months of daily usage I decided to get rid of my stripboard and order a pcb that I designed. Its very cheap in various online stores. The schematics can be directly transformed into a pcb design and the gerber file for production is directly uploaded to a manufacturer. 
Two or three weeks later I had my newly designed pcb in hand, soldered the components, tinkered with some new errors and issues (PCB design and bad battery connectors) and packed it in a dry wall box with the battery. 
Now my procedure has changed: I enter our main door and ring my own doorbell. After walking up one floor the esp is awake, connected to my wifi and ready to read my fingerprint, the nuki opens the door to me and my phone can stay in my jacket. But: I still have the key in my pocket!

Entities involved
- Nuki Smart Lock Pro Gen4
- R503 grow fingerprint reader
- Siedle doorbell (6+n system) 
- ESP32-board - ESP32-S3-Zero (Waveshare) with Charger and fuel gauge (see ressources)
- LiPo battery 3,7 V 4000mAh
- Homeassistant running on a Pi5

Schematics and PCB
- picture 
- picture
- old assembly stripboard
- new pcb

Various impressions
- picture r503 in case
- 


Help and ressources
- ESP32-S3-Zero Waveshare
    https://www.waveshare.com/wiki/ESP32-S3-Zero
- R503 ESPHome
    https://esphome.io/components/fingerprint_grow.html
- 3d printed case for the reader
    https://www.thingiverse.com/thing:5419130/files
- IObroker Siedle Doorbell - discussion around the Siedle intercom with some very helpful posts (in German)
    https://forum.iobroker.net/topic/7660/siedle-klingel-t%C3%BCr%C3%B6ffner-mit-esp8266-und-mqtt
- easyeda - design circuits and pcbs online
    https://easyeda.com/
- Battery charger and boost converter
    https://www.wemos.cc/en/latest/d1_mini_shield/battery.html
- Battery fuel gauge via I2C
    https://www.sparkfun.com/sparkfun-lipo-fuel-gauge.html
- 3V3 Relay circuit schematic from an off the shelf relay board
    https://www.reddit.com/media?url=https%3A%2F%2Fpreview.redd.it%2Fschematic-review-of-esp32-relay-board-v0-r95h7sixj8yc1.png%3Fwidth%3D2382%26format%3Dpng%26auto%3Dwebp%26s%3D9724f92cff9422d24422ecfdb0569f7d5bcf47b2


homeassistant
- ESPHome integrates the data from the fingerprint reader and from the intercom
- the fuel gauge connected to the battery sends a battery charge percentage and a voltage to estimate the longevity 
- automations in HA open the door via nuki or use the buzzer to enter the building


improvements
- lots of them!
- debug pins missing on the pcb
- sometimes my doorbell recognized a ring but the ring of my neighbourgs was used - my circuit could be too sensitive on the doorbell lines
- improve the wakeup speed -> homeassistant connection eg. via bluetooth, wifi can be slow to login, wifi 6 can be faster!? 
- proximity wakeup or anything like this for the future 
- use some power from the intercom system to charge the battery?
