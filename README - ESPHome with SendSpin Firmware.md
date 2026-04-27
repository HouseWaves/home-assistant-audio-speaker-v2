# Moving from Squeezelite to ESPHome with SendSpin for V2 - DIY Wi-Fi & BT speaker for Home Assistant audio;  



### This guide walks DIY Home Assistant audio builders through installing ESPHome firmware with the SendSpin streaming protocol on the Sonocotta ESP32-LOUD board.



### WARNING - If you install ESPHome (or any other firmware) over existing  Squeezelite firmware, it will be trickier to fall back and reinstall Squeezelite.

### Reinstalling Squeezelite over existing firmware REQUIRES pressing a small reset button on the ESP32 circuit board, as you start the install process.  If you have already installed the ESP32 inside the speaker cabinet, you may have to remove the speaker driver, and possibly pull out the ESP32 board again, to reach the reset button with your finger.  



## See and Hear it

- [VIDEO - SIDE BY SIDE COMPARISON OF STREAMING with SQUEEZELITE vs SENDSPIN](https://www.youtube.com/watch?v=yusFQKQjKN4)
  The main thing to pick out is the first three seconds on playback using SendSpin: the audio will start/stop several times as the streaming management begin playback quickly (DAC turned on within 160ms) while it struggles to keep up with all the tasks underway (getting packets, negotiating the handshake, management of time synchronization, etc). 
  
  

---

## Table of Contents

- [What This Is](#what-this-is)
- [Motivation](#motivation)
- [Caveats & Limitations](#caveats--limitations)
- [Tools Required](#tools-required)
- [Build Time](#build-time)
- [Step-by-Step Build Guide](#step-by-step-build-guide)
- [What's Next](#whats-next)
- [Credits & References](#credits--references)

---

## What This Is 

- A documented process to guide users through the process of using ESPHome Device Builder to compile and install ESPHome with SendSpin for use ONLY on the Sonocotta ESP32-LOUD controller.



---

## Motivation

Many, many people (myself included) are excited about the emergence of ESPHome's SendSpin streaming protocol, both for the use of grouping multiple speakers (players) for multi-room music synchronization and also for adding smart speaker functionality.

Thankfully, Andriy at Sonocotta has provided a full range of tested and working firmware options to use on his ESP32 boards.

Although the SendSpin protocol is still under development, it has progressed to a level where people are willing to begin using it while the remaining bugs are worked out.



---

## Caveats & Limitations

- **IMPORTANT -  {as of April 26, 2026} - in my personal testing, streaming music with SendSpin is not perfect. I experience stutters during the first 3 seconds of beginning a new audio track.  I also experience multiple microsecond drop outs every minute during playback. Users should consider if they are willing to live with these issues before switching.  Users wanting a flawless experience should probably remain with Squeezelite for awhile longer.**

- This guide was created APRIL 2026 for using Home Assistant 2026.4.4 with ESPHome 2026.4.2 

  I have no doubt the screens and steps outlined below will change quickly as Home Assistant continues to evolve, so the steps below may no longer be identical at the time you are using this guide.
  
- **THIS GUIDE IS EXTREMELY VERBOSE -- IT IS EXTREMELY LONG**

- ***This is on purpose, as I initially struggled when I first used ESPHome Device Builder.  I wanted to create a comprehensive and thorough set of instructions with step-by-step screen shots to hand-hold beginners on their first time using the tool.***  

  Once you use ESPHome Device Builder in a guided fashion, it will quickly become familiar and approachable to use on a regular basis.
  
  


------

## Tools Required

- Computer with access to Home Assistant and a USB-C cable 
- ESPHome Device Builder installed - this is an add-on (APP) available in Home Assistant (step 1 if you need it)



---

## Build Time

- Approximately 25 minutes if running HA on a Raspberry Pi 4; Slower for earlier models, faster for NUCs. The bulk of this time will be waiting while HA compiles and installs the firmware. It takes my RPi approx 14 minutes.



---

## Step-by-Step Installation Guide



### Step 1 

###  — Ensure you have ESPHome Device Builder installed in HA

This is a Home Assistant application that onboards and prepares ESP32  (and other) devices for use with Home Assistant.

1. In Home Assistant, Click on Settings, then Click on Apps

2. If you do not see the ESPHome Device Builder on this page, use the blue button at the bottom to install it

3. type in ESPHome Device... in the search bar

4. Select the one with no additional names (not Experimental, not Developer)...just plain "ESPHome Device Builder"

   ![1_sendspin_esphome_esphome_device_builder_housewaves_and_loud_esp32](images-senspin-install/1_sendspin_esphome_esphome_device_builder_housewaves_and_loud_esp32.jpg)






### Step 2 

###  — Install ESPHome (starter) firmware 



1. Open the ESPHome web-based firmware installer in a browser:  https://web.esphome.io/  

2. Connect the ESP32 LOUD board to your computer with the USB-C cable

3. In the ESP Device box on the screen, Click on Connect and select the serial connection in the pop up box.

   ![2_sendspin_esphome_prepare_for_housewaves_and_loud_esp32](images-senspin-install/2_sendspin_esphome_prepare_for_housewaves_and_loud_esp32.jpg)

4. Click on "Prepare For First Use"

   ![3_sendspin_esphome_basic_firmware_install_for_housewaves_and_loud_esp32](images-senspin-install/3_sendspin_esphome_basic_firmware_install_for_housewaves_and_loud_esp32.jpg)

5. Wait for the firmware to install

6. When finished **be sure to write down the new device name - you will need it later**   

    In this example, the device name is "esphome-web-9cc7bc"

   ![4_sendspin_esphome_save_entity_id_housewaves_and_loud_esp32](images-senspin-install/4_sendspin_esphome_save_entity_id_housewaves_and_loud_esp32.jpg)

7. Enter your home Wi-Fi credentials

8. Click Connect

   ![5_sendspin_esphome_connect_network_housewaves_and_loud_esp32](images-senspin-install/5_sendspin_esphome_connect_network_housewaves_and_loud_esp32.jpg)

9. When HA connects to the device, you have completed the Initial Provisioning

10. You may Click CLOSE inn the dialog box

11. Keep the web browser open for the next step

    ![6_sendspin_esphome_speaker_provisioned_housewaves_and_loud_esp32](images-senspin-install/6_sendspin_esphome_speaker_provisioned_housewaves_and_loud_esp32.jpg)



### Step 3

###  — Download the YAML code needed for SendSpin

ESPHome Device Builder will need a configuration file used in building the firmware. Luckily Sonocotta keeps these files already prepared in a GitHub repository.  Other than a few small edits, everything is ready for you.

Please note - every board has several YAML configuration file options available depending on firmware needed

The link below is specifically for the ESP32-LOUD board chosen for the V2 DIY instructions in this repo.  If you chose another board (e.g. the S3 version, or maybe the LOUDER board, you will need to use the corresponding file.)

1. Open Sonocotta's GitHub repo at:  https://github.com/sonocotta/esp32-audio-dock/tree/main/firmware/esphome

2. Download the YAML configuration file for ESP32-LOUD-SENDSPIN:  https://github.com/sonocotta/esp32-audio-dock/blob/main/firmware/esphome/2-loud-esp32/loud-esp32-idf-sendspin.yaml

   ![sendspin_esphome_yaml_for_housewaves_and_loud_esp32](images-senspin-install/sendspin_esphome_yaml_for_housewaves_and_loud_esp32.jpg)

3. Open in a text editor (e.g. Notepad in Windows)

   ![13_sendspin_esphome_builder_edit_yaml_substitutions_housewaves_and_loud_esp32](images-senspin-install/13_sendspin_esphome_builder_edit_yaml_substitutions_housewaves_and_loud_esp32.jpg)

4. Edit three lines

   a.  in the first section labeled "substitutions", change the first line to the name you wrote down from step 1

     I have already changed it to *name: esphome-web-9cc7bc*   (esphome-web-9cc7bc was the name assigned in step 1)

     You will have a slightly different suffix

   b.  you may optionally change the friendly-name in the line below

   c.  scroll down to the line with logger_level.  You should change this to *WARN* or *INFO*

   ​    DEBUG will flood your log file with a lot of unnecessary information.

5. **SAVE this file for the future!**   

   

### Step 4

###  — Prepare ESPHome Device Builder to install the firmware

1. Open ESPHome Device Builder

2. Click on SECRETS in the top right corner

   ![10_sendspin_esphome_builder_start_screen_housewaves_and_loud_esp32](images-senspin-install/10_sendspin_esphome_builder_start_screen_housewaves_and_loud_esp32.jpg)

3. Add the top 4 lines to the secrets.yaml file, replacing everything in the quotes with your network ssid and password

   the ota_password is a new password you will create; keep this saved somewhere

     **NOTE - this SECRETS file is different than the secrets yaml file created in HA Configuration**

   ![11_sendspin_esphome_builder_save_secrets_housewaves_and_loud_esp32](images-senspin-install/11_sendspin_esphome_builder_save_secrets_housewaves_and_loud_esp32.jpg)

4. Click SAVE

5. in the black bar across the top, it should say it has discovered your new advice - Click SHOW on the far right side

   ![10_sendspin_esphome_builder_start_screen_housewaves_and_loud_esp32](images-senspin-install/10_sendspin_esphome_builder_start_screen_housewaves_and_loud_esp32.jpg)

6. Your new controller will have it's own box

   ![12_sendspin_esphome_builder_show_discovered_device_housewaves_and_loud_esp32](images-senspin-install/12_sendspin_esphome_builder_show_discovered_device_housewaves_and_loud_esp32.jpg)

7. Click "TAKE CONTROL"

8. Acknowledge the warnings and Click TAKE CONTROL in the new popup box

   ![15_sendspin_esphome_builder_take_control_housewaves_and_loud_esp32](images-senspin-install/15_sendspin_esphome_builder_take_control_housewaves_and_loud_esp32.jpg)

9. in the next popup box "Configuration Created" - **Do NOT Click INSTALL - Click SKIP instead**

   ![14_sendspin_esphome_builder_ready_to_install_housewaves_and_loud_esp32](images-senspin-install/14_sendspin_esphome_builder_ready_to_install_housewaves_and_loud_esp32.jpg)

10. You will return to the main window

11. You new device will now have TWO options on the bottom: EDIT and LOGS

    ![16_sendspin_esphome_builder_edit_device_housewaves_and_loud_esp32](images-senspin-install/16_sendspin_esphome_builder_edit_device_housewaves_and_loud_esp32.jpg)

12. Click EDIT

13. The new screen will show up with basic YAML configuration code already prepared - we will not need this.

14. DELETE ALL the lines in this yaml file

    ![17.1_sendspin_esphome_builder_copy_paste_yaml_housewaves_and_loud_esp32](images-senspin-install/17.1_sendspin_esphome_builder_copy_paste_yaml_housewaves_and_loud_esp32.jpg)

15. Go back to your text editor with the YAML file you created in STEP 3

16. COPY ALL lines from that YAML file

17. PASTE ALL lines into the ESPHome screen

    ![17.2_sendspin_esphome_builder_copy_paste_yaml_housewaves_and_loud_esp32](images-senspin-install/17.2_sendspin_esphome_builder_copy_paste_yaml_housewaves_and_loud_esp32.jpg)

18. Click SAVE in the upper right corner

19. Click INSTALL in the upper right corner

20. Click Wirelessly in the new popup box

    ![18_sendspin_esphome_builder_install_wirelessly_housewaves_and_loud_esp32](images-senspin-install/18_sendspin_esphome_builder_install_wirelessly_housewaves_and_loud_esp32.jpg)

21. WHEW --- Your work is done;  Time for HA to compile and install the firmware

22. The Log screen will pop up and you'll see the screen quickly fill to the last line  "Reading CMAKE Configuration...."

23. **This is when everything gets really, really slow, do not be alarmed if nothing changes for 5 or more minutes as the compiler begins preparing.**

    ![19.0_sendspin_esphome_builder_log_screen_compiling_starts_housewaves_and_loud_esp32](images-senspin-install/19.0_sendspin_esphome_builder_log_screen_compiling_starts_housewaves_and_loud_esp32.jpg)

24. After what seems like an eternity, you will see the screen begin to fill up with hundreds of lines as it reads each component of the code library.

25. When it finishes (see the second to last line -  TOOK 920.17 SECONDS ) you will see the summary report

26. Several lines in this summary should start with "Successfully created..."

    ![19.2_sendspin_esphome_builder_log_screen_compiling_success_housewaves_and_loud_esp32](images-senspin-install/19.2_sendspin_esphome_builder_log_screen_compiling_success_housewaves_and_loud_esp32.jpg)

27. If everything works, your device will reboot and you'll see a screen similar to below.

    NOTE - if you changed the logger_level from DEBUG to something else, you will not see most of this

    But, you should see the messages showing SendSpin is up and running.

    OPTIONAL - You may want to keep this screen open - while you open another window with Music Assistant to start using your new SendSpin speaker.  It can be interesting to follow the status messages as you begin to stream audio.

    ![19.3_sendspin_esphome_builder_log_screen_sendspin_speaker_ready_housewaves_and_loud_esp32](images-senspin-install/19.3_sendspin_esphome_builder_log_screen_sendspin_speaker_ready_housewaves_and_loud_esp32.jpg)

28. When you're finished - Click STOP and your done.



---

## What's Next

I had to move up the ESPHome SendSpin firmware test and update to get ready for building Smart Speakers this summer.  So I pushed back the V3 speaker post.  It's next - maybe next week, but definitely in two weeks. 

Even better news, is I completed the SendSpin testing for both V2 and V3 at the same time, so when I do post in 1-2 weeks, you will be able to build and install SendSpin immediately (or Squeezelite)



| Build | Speaker                                                      | Status           |
| ----- | ------------------------------------------------------------ | ---------------- |
| #1    | Tozzi One + Sonocotta Louder ESP32                           | ✅ Complete       |
| #2    | HouseWaves-One; Low-end, low-cost (sub $50) single driver speaker | ✅ Complete       |
| #3    | SendSpin firmware guide - for use with DIY builds V2 and V3 (aka HW-One and HW-Two) | ✅ Complete       |
| #4    | Mid-range, mid-cost (sub$100), 2-way speaker (aka HouseWaves-Two) | 🔜 May 2026       |
| #5    | PoE powered ESP32 for in-wall and in-ceiling speakers        | 🔜 June 2026      |
| #6    | Value-priced, (sub $200) Sonos-starter alternative for Home Assistant | 🔜 June/July 2026 |
| #7    | Subwoofer (may change priority with smart speaker)           | 🔜 July 2026      |
| #8    | Smart Speaker (may change priority with subwoofer)           | 🔜 July 2026      |



**Features planned for future builds:**

- Open-Source, Smart-speaker option, fully compatible with Home Assistant Voice Preview (HouseWaves-SmartVP)
- Additional power options - e.g. Power over Ethernet (PoE)
- Option to pair a second passive speaker with 1 wire (HouseWaves-DUO).
- DAC & Amplifier upgrades, DSP options, higher power speakers (ESP32 Plus)



---

## Credits & References

- [Sonocotta Loud ESP32 Documentation](https://github.com/sonocotta/loud-esp)

- [Sonocotta ESP32 Audio Dock Reference](https://github.com/sonocotta/esp32-audio-dock)

- [ESPHome Web-based Firmware Installer](https://web.esphome.io/)

- [Music Assistant for Home Assistant](https://music-assistant.io/)

- [Home Assistant Media Player Integration](https://www.home-assistant.io/integrations/media_player/)

  

---

***Build dated 2026-04-26| HouseWaves, Smarter Home Audio.  Copyright, 2026.***

