# MrDiy Audio Notifier - Schmurtz Edition
 ----
  MrDiy Audio Notifier is an audio player controlled by MQTT.

  MrDiy Audio Notifier is based on esp8266audio library. This repo uses most of the MrDiy's code with some modifications :

- Ported to platformio (with differents recommanded settings for ESP8266audio)
- Can be compiled for ESP8266 and ESP32
- Google Translate TTS (so a little less "cloudless" but multilingual and better voice than the included ESP8266SAM)
- ability to play AAC (required for many web radio) and flac (not tested)
- This code also allows to switch easily between no DAC (version used by mr DIY) , external DAC or internal DAC (for ESP32).
- Documentation to connect your ESP to a speaker (see comments at the top of the "main.cpp" file)
- You’ll also find some useful comments for wiring your DAC quickly or to improve the code (sound level of RTTTS, ssl, IotWebConf migration to v3.x...).

 How to use :
 ----
 Check the comments at the beginning of the "main.cpp".

 MQTT COMMANDS:  ( 'your_custom_mqtt_topic' is the MQTT prefix defined during the setup)

      - Play an MP3             MQTT topic: "your_custom_mqtt_topic/play"
                                MQTT load: http://url-to-the-mp3-file/file.mp3
                                PS: supports HTTP only, no HTTPS. -> this could be a solution :
                                https://github.com/earlephilhower/ESP8266Audio/pull/410

      - Play an AAC             MQTT topic: "your_custom_mqtt_topic/aac"  (better for ESP32, hard for esp8266)
                                MQTT load: http://url-to-the-aac-file/file.aac

      - Play an Icecast Stream  MQTT topic: "your_custom_mqtt_topic/stream"
                                MQTT load: http://url-to-the-icecast-stream/file.mp3
                                example: http://22203.live.streamtheworld.com/WHTAFM.mp3

      - Play a Ringtone         MQTT topic: "your_custom_mqtt_topic/tone"
                                MQTT load: RTTTL formated text
                                example: Soap:d=8,o=5,b=125:g,a,c6,p,a,4c6,4p,a,g,e,c,4p,4g,a

      - Say Text                MQTT topic: "your_custom_mqtt_topic/say"
                                MQTT load: Text to be read
                                example: Hello There. How. Are. You?

      - Stop Playing            MQTT topic: "your_custom_mqtt_topic/stop"

      - Change the Volume       MQTT topic: "your_custom_mqtt_topic/volume"
                                MQTT load: a double between 0.00 and 1.00
                                example: 0.7

      - Say Text with Google    MQTT topic: "your_custom_mqtt_topic/tts"
                                MQTT load: Text to be read
                                example: Hello There. How. Are. You?



 How to connect ESP to speaker :
 ----
3 possibilities to have a sound output from the ESP (please read [ESP8266Audio](https://github.com/earlephilhower/ESP8266Audio) documentation for more information): 
- use an additional I2S DAC module (like PCM5102 for example, MAX98357 board offers a 3W amp to drive your speaker directly)
- use the internal DAC of the ESP32 (not available on ESP8266)
- use a virtual DAC sigma (the sound will be less good than a DAC but enough for speak notifications)

Don't try to drive the speaker directly, the ESP8266 pins can't give enough current to drive even a headphone well and you may end up damaging your device. Instead use a simple transistor 2N2222 or 2N3904 and ~1K resistor:

```
                            2N3904 (NPN)
                            +---------+
                            |         |     +-|
                            | E  B  C |    / S|
                            +-|--|--|-+    | P|
                              |  |  +------+ E|
                              |  |         | A|
ESP8266-GND ------------------+  |  +------+ K| 
                                 |  |      | E|
ESP8266-I2SOUT (Rx) -----/\/\/\--+  |      \ R|
                                    |       +-|
USB 5V -----------------------------+

You may also want to add a 220uF cap from USB5V to GND 
just to help filter out any voltage droop during high volume playback.
```

 Some issues for now :
 ----
 - Playing GoogleTTS crash at end (for both ESP8266 and ESP32) : [issue #327](https://github.com/earlephilhower/ESP8266Audio/issues/327)
 - <strike>Playing RTTTL (nokia tone) never stop playing the last note (ESP32 only) :</strike> [issue #395 resolved](https://github.com/earlephilhower/ESP8266Audio/issues/395)
 
 These issues are related to [ESP8266audio library](https://github.com/earlephilhower/ESP8266Audio) so it has been created on their repo ;)
 
 
 
 
 
 ===============================================================================
 #  Original description from MrDIY 
 https://gitlab.com/MrDIYca/mrdiy-audio-notifier

In this project, I will show you how you can use an ESP8266 module or board like the Wemos D1 Mini to play MP3, TTS and RTTTL. It is controlled over MQTT. You will be able to send it an MQTT message with the URL of the MP3 file and it will play it for it. It is also capable to doing basic TTS and playback RTTTL (aka Nokia) ringtone.
<br><br><br>
**Watch the video**: click on the image below:

[![MrDIY Audio Notifier youtube video](https://img.youtube.com/vi/SPa9SMyPU58/0.jpg)](https://www.youtube.com/watch?v=SPa9SMyPU58)


## Instructions

Please visit <a href="https://www.instructables.com/id/MQTT-Audio-Notifier-for-ESP8266-Play-MP3-TTS-RTTL">my Instructables page</a> page for full instructions or watch the video above.

<p>Don't forget to check out <a href="https://www.youtube.com/channel/UCtfYdcn8F8wfRA2BXp2FPtg">my YouTube channel</a>  and follow me on <a href="https://twitter.com/MrDIYca">Twitter</a>. If you are intersted in supporting my work, please visit <a href="https://www.patreon.com/MrDIYca?fan_landing=true">my Patreon page</a>.</p>


## Thanks
Many thanks to all the authors and contributors to the open source libraries I have used. You have done an amazing job for the community!

