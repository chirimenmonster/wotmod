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

version 0.9.17.1 以降を対象としたサウンド mod の作成方法です。
シンプルかつ短時間で作成できるようにしました。
主な特長は、現在あるコンテナ全体を入れ替えずに、特定のサウンドファイルを置き換えることが可能であるということです。


Due to the fact that all works are handled in Wwise environment,
we suggest visiting their official YouTube channel
([https://www.youtube.com/user/AudiokineticWwise](https://www.youtube.com/user/AudiokineticWwise]))
to pass relevant quick tutorial.

すべての操作を Wwise 内で行うので、
公式 YouTube チャンネル
([https://www.youtube.com/user/AudiokineticWwise](https://www.youtube.com/user/AudiokineticWwise]))
の関連するクイックチュートリアルに目を通しておくことをおすすめします。


1. Download and install Wwise [https://www.audiokinetic.com/download/](https://www.audiokinetic.com/download/),
version 2017.1.1 (note - this specific version is mandatory!)  
WWISE version 2017.1.1 をダウンロードしてインストールします
[https://www.audiokinetic.com/download/](https://www.audiokinetic.com/download/)。
(注: このバージョンでなければなりません!)


2. Open the project:  
プロジェクトを開きます:
![Open the project](/resources/image_20171105_01.png)
Click `Open Other` in the opened window, then choose `WoT_sound_mod_version_<game_version>.wproj`  
開いたウィンドウ内の [**Open Other**] をクリックして、
`WoT_sound_mod_version_<game_version>.wproj` を選択します。
![Click Open Other in the opened window](/resources/image_20171105_02.png)

3. Proceed to `Audio tab`  
[**Audio**] タブに進みます。
![Proceed to Audio tab](/resources/image_20171105_03.png)

4. Drag the desired sound files into `Actor-Mixer Hierarchy` -> `Default Work Unit` to replace the old ones  
対象のサウンドファイルを元のファイルと置き換えるため、
[**Actor-Mixer Hierarchy**] -> [**Default Work Unit**] にドラッグします。  
![Drag the desired sound files](/resources/image_20171105_04.png)

5. In the window opened you’ll see the imported files.
Please check that `Import as` has `Sound SFX` feature selected (please don't mistakenly select `Sound Voice`).  
ウィンドウが開いてインポートしたファイルが表示されます。
[**Import as**] の [**Sound SFX**] が選択されていることを確認してください
(間違って [**Sound Voice**] を選択しないようにしてください)。  
![check that Import](/resources/image_20171105_05.png)

6. To make things easy, you may rename the files via pressing F2 key.
Add «\_mod» for example, to get *wpn_mod*  
作業をやりやすくするために、F2 キーでファイル名を変更できます。
この例では «\_mod» を追加して `wpn_mod` としました。
![rename the files](/resources/image_20171105_06.png)

7. In `Out Bus` section you have to choose the bus where we’ll have our sound directed to (bus structure description is available in bus Notes).
Correct selection is necessary for the volume tweaking by appropriate sliders (Interface, Vehicles, Voices etc) in the game settings.
By default, the sound will be directed into `Master Audio Bus` - i.e. it will be responsive to the master volume slider.  
[**Output Bus**] セクションで、追加したサウンドが送られるバスを選択する必要があります
(バスの仕組みの詳細については bus Notes を参照してください)。
ゲーム設定の対応するスライダ (インターフェース、車輌、音声など) で音量を調整するには、正しいものを選択する必要があります。
デフォルトではサウンドは [**Master Audio Bus**] に送られます。
つまり、主音量のスライダに対応するということになります。  
![rename the files](/resources/image_20171105_07.png)

8. Proceed to `Source Settings` tab and define conversion settings (Vorbis, Quality High).  
[**Source Settings**] タブに進み、変換設定 (conversion settings) を定義します (Vorbis, Quality High)。  
![Proceed to Source Settings tab and define conversion settings](/resources/image_20171105_08.png)
Press `Edit` then to define the desired quality by dragging the slider.
The higher the quality set – the bigger will be the size of the resulting soundbank.
We suggest using values between 4 and 6.  
[**Edit**] を押し、スライダをドラッグして希望の音質に設定します。
音質が高品質であるほどサウンドバンクのサイズは大きくなります。
4～6くらいがよいでしょう。  
![Press Edit then to define the desired quality by dragging the slider](/resources/image_20171105_09.png)

9. Proceed to the `Events` tab.
Here, you’ll have to find the event you want to be replaced;
please pay attention to the `Notes` field that contains descriptions of the events.
Add a “play” rule to `wpn_huge_PC_mod event`, then right click on the rule and select `Browse`.
In the dialog window opened you need to find the required sound file (`wpn_mod`).  
[**Events**] タブに進みます。
ここは置き換えたいイベントを見つける必要があります。
イベントの説明が記載されている [**Notes**] 欄に注意してください。
`wpn_huge_PC_mod event` に “play” ルールを追加し、
ルール上で右クリックして [**Browse**] を選択します。
開いたダイアログの中に、必要なサウンドファイル (`wpn_mod`) を見つける必要があります。　　
![Proceed to the Events tab](/resources/image_20171105_10.png)

10. Open the `SoundBanks` tab.
Create new `.bnk` file in the `Default Work Unit` (for example, call it `mod`).  
[**SoundBanks**] タブを開きます。
新規の `.bnk` ファイルを [**Default Work Unit**] 内に作成します
(この例では `mod` とします)。  
![Create new .bnk file](/resources/image_20171105_11.png)

11. Press F7 and proceed to `SoundBank Manager`.
Drag the event (`wpn_huge_PC_mod`) into `Hierarchy Inclusion` field (note – checkboxes for `Events`, `Structures`, `Media` should be on)  
F7 を押して [**SoundBank Manager**] に進みます。
イベント (`wpn_huge_PC_mod`) を [**Hierarchy Inclusion**] 欄にドラッグします
(注: [**Events**], [**Structures**], [**Media**] のチェックがオンになっている必要があります)。  
![Drag the event](/resources/image_20171105_12.png)
Enable all necessary checkboxes in `Platforms` and `Languages` groups.  
`Platforms` と `Languages` グループの必要なチェックボックスを全て有効にします。

12. Proceed to the `User Settings`.
Enable the `Override Project SoundBank Paths` checkbox and define the path for the .bnk to generate.  
[**User Settings**] に進みます。
[**Override Project SoundBank Paths**] チェックボックスを有効にし、
生成する `.bnk` のパスを定義します。
![Proceed to the User Settings](/resources/image_20171105_13.png)

13. Once the .bnk generated, click `Close`.  
`.bnk` が生成されたら [**Close**] をクリックします。  
![Once the .bnk generated](/resources/image_20171105_14.png)

14. Create folder `<game_folder>/res_mods/<game_version>/audioww/`,
сopy the generated `mod.bnk` there both with `audio_mods.xml` provided in the same archive with the project.  
フォルダ `<game_folder>/res_mods/<game_version>/audioww/` を作成し、
生成された `mod.bnk` と、プロジェクトと同じ場所に提供されている `audio_mods.xml` をコピーします。

15. Open `audio_mods.xml` and specify there our `mod.bnk`  
`audio_mods.xml` を開き、作成した `mod.bnk` を指定する設定をします。  
![Open audio_mods.xml and specify there our mod.bnk](/resources/image_20171105_15.png)  
Add default event `obj_bicycle` and event, that should be replaced by `obj_bicycle_mod`  
デフォルトイベント `obj_bicycle` を追加します。
このイベントは `obj_bicycle_mod` で置き換えられるイベントです。  
![Add default event obj_bicycle and event](/resources/image_20171105_16.png)  
(if using `SWITCHES`, `STATES`, `RTPC` make sure they’re added in relevant strings)  
(`SWITCHES`, `STATES`, `RTPC` を使用している場合は、関連する文字列に追加されていることを確認してください)  
![Add default event obj_bicycle and event](/resources/image_20171105_17.png)  
![Add default event obj_bicycle and event](/resources/image_20171105_18.png)
![Add default event obj_bicycle and event](/resources/image_20171105_19.png)  
It is time to save your `audio_mods.xml`  
以上で `audio_mods.xml` を保存します。

16. The mod is good to go ☺.  
mod の準備ができました☺。
