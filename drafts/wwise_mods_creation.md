---
layout: post
title: "Wwise mods creation"
excerpt: "Wwise mods creation の翻訳"
date: 2017-11-05 21:00 +0900
---
原文:
[Wwise mods creation.pdf](https://koreanrandom.com/forum/applications/core/interface/file/attachment.php?id=118868)

# Creation of sound mods for World of Tanks

Starting from version 0.9.17.1 we’re introducing an option for you to create and enable custom sound mods in the game.
We tried our best to keep the process as simple and quick as possible.
One of its main advantages is that you’ll be allowed to replace any specific sound file
without the necessity to replace the whole container from now on.

Due to the fact that all works are handled in Wwise environment,
we suggest visiting their official YouTube channel
([https://www.youtube.com/user/AudiokineticWwise](https://www.youtube.com/user/AudiokineticWwise]))
to pass relevant quick tutorial.

1. Download and install Wwise [https://www.audiokinetic.com/download/](https://www.audiokinetic.com/download/),
version 2017.1.1 (note - this specific version is mandatory!)

2. Open the project:
![Open the project](/resources/image_20171105_01.png)
Click `Open Other` in the opened window, then choose `WoT_sound_mod_version_<game_version>.wproj`
![Click Open Other in the opened window](/resources/image_20171105_02.png)

3. Proceed to `Audio tab`
![Proceed to Audio tab](/resources/image_20171105_03.png)

4. Drag the desired sound files into `Actor-Mixer Hierarchy` -> `Default Work Unit` to replace the old ones
![Drag the desired sound files](/resources/image_20171105_04.png)

5. In the window opened you’ll see the imported files.
Please check that `Import as` has `Sound SFX` feature selected (please don't mistakenly select `Sound Voice`).
![check that Import](/resources/image_20171105_05.png)

6. To make things easy, you may rename the files via pressing F2 key.
Add «\_mod» for example, to get *wpn_mod*
![rename the files](/resources/image_20171105_06.png)

7. In `Out Bus` section you have to choose the bus where we’ll have our sound directed to (bus structure description is available in bus Notes).
Correct selection is necessary for the volume tweaking by appropriate sliders (Interface, Vehicles, Voices etc) in the game settings.
By default, the sound will be directed into `Master Audio Bus` - i.e. it will be responsive to the master volume slider.
![rename the files](/resources/image_20171105_07.png)

8. Proceed to `Source Settings` tab and define conversion settings (Vorbis, Quality High).
![Proceed to Source Settings tab and define conversion settings](/resources/image_20171105_08.png)
Press `Edit` then to define the desired quality by dragging the slider.
The higher the quality set – the bigger will be the size of the resulting soundbank.
We suggest using values between 4 and 6.
![Press Edit then to define the desired quality by dragging the slider](/resources/image_20171105_09.png)

9. Proceed to the `Events` tab.
Here, you’ll have to find the event you want to be replaced;
please pay attention to the `Notes` field that contains descriptions of the events.
Add a “play” rule to `wpn_huge_PC_mod event`, then right click on the rule and select `Browse`.
In the dialog window opened you need to find the required sound file (`wpn_mod`).
![Proceed to the Events tab](/resources/image_20171105_10.png)

10. Open the `SoundBanks` tab.
Create new `.bnk` file in the `Default Work Unit` (for example, call it `mod`).
![Create new .bnk file](/resources/image_20171105_11.png)

11. Press F7 and proceed to `SoundBank Manager`.
Drag the event (`wpn_huge_PC_mod`) into `Hierarchy Inclusion` field (note – checkboxes for `Events`, `Structures`, `Media` should be on)
![Drag the event](/resources/image_20171105_12.png)
Enable all necessary checkboxes in `Platforms` and `Languages` groups.

12. Proceed to the `User Settings`.
Enable the `Override Project SoundBank Paths` checkbox and define the path for the .bnk to generate.
![Proceed to the User Settings](/resources/image_20171105_13.png)

13. Once the .bnk generated, click `Close`.
![Once the .bnk generated](/resources/image_20171105_14.png)

14. Create folder `<game_folder>/res_mods/<game_version>/audioww/`,
сopy the generated `mod.bnk` there both with `audio_mods.xml` provided in the same archive with the project.

15. Open `audio_mods.xml` and specify there our `mod.bnk`  
![Open audio_mods.xml and specify there our mod.bnk](/resources/image_20171105_15.png)  
Add default event `obj_bicycle` and event, that should be replaced by `obj_bicycle_mod`
![Add default event obj_bicycle and event](/resources/image_20171105_16.png)  
(if using `SWITCHES`, `STATES`, `RTPC` make sure they’re added in relevant strings)
![Add default event obj_bicycle and event](/resources/image_20171105_17.png)  
![Add default event obj_bicycle and event](/resources/image_20171105_18.png)
![Add default event obj_bicycle and event](/resources/image_20171105_19.png)  
It is time to save your `audio_mods.xml`

16. The mod is good to go ☺.
