---
layout: post
title: "Wwise mods creation"
excerpt: "Wwise mods creation の翻訳"
date: 2017-11-06 20:30 +0900
---
原文:
[Wwise mods creation.pdf](/resources/Wwise mods creation.pdf)  
関連:
[Korean Random: wwise project for 9.20.1](https://koreanrandom.com/forum/topic/41341-wwise-project-for-9201/)

# World of Tanks サウンド mod の作り方 

version 0.9.17.1 以降を対象としたサウンド mod の作成方法です。
極力シンプルかつ短時間で作成できるようにしました。
主な特長は、現在あるコンテナ全体を入れ替えずに、特定のサウンドファイルを置き換えることが可能になっている点です。

すべての操作を Wwise 内で行うので、
公式 YouTube チャンネル
([https://www.youtube.com/user/AudiokineticWwise](https://www.youtube.com/user/AudiokineticWwise]))
の関連するクイックチュートリアルに目を通しておくことをおすすめします。


1. Wwise version 2017.1.1 をダウンロードしてインストールします
[https://www.audiokinetic.com/download/](https://www.audiokinetic.com/download/)。
(注: このバージョンでなければなりません!)


2. プロジェクトを開きます:
![Open the project](/resources/image_20171105_01.png)
開いたウィンドウ内の [**Open Other**] をクリックして、
`WoT_sound_mod_version_<game_version>.wproj` を選択します。
![Click Open Other in the opened window](/resources/image_20171105_02.png)

3. [**Audio**] タブに進みます。
![Proceed to Audio tab](/resources/image_20171105_03.png)

4. 置き換えたいサウンドファイルを
[**Actor-Mixer Hierarchy**] -> [**Default Work Unit**]
にドラッグします。  
![Drag the desired sound files](/resources/image_20171105_04.png)

5. ウィンドウが開いてインポートしたファイルが表示されます。
[**Import as**] の [**Sound SFX**] が選択されていることを確認してください
(間違って [**Sound Voice**] を選択しないようにしてください)。  
![check that Import](/resources/image_20171105_05.png)

6. 作業をやりやすくするために、F2 キーでファイル名を変更できます。
この例では «\_mod» を追加して `wpn_mod` としました。
![rename the files](/resources/image_20171105_06.png)

7. [**Output Bus**] セクションで、追加したサウンドが送られるバスを選択する必要があります
(バスの仕組みの詳細については bus Notes を参照してください)。
ゲーム設定の対応するスライダ (インターフェース、車輌、音声など) で音量を調整するには、正しいものを選択する必要があります。
デフォルトではサウンドは [**Master Audio Bus**] に送られます。
つまり、主音量のスライダに対応するということになります。  
![rename the files](/resources/image_20171105_07.png)

8. [**Source Settings**] タブに進み、変換設定 (conversion settings) を定義します (Vorbis, Quality High)。  
![Proceed to Source Settings tab and define conversion settings](/resources/image_20171105_08.png)  
[**Edit**] を押し、スライダをドラッグして希望の音質に設定します。
音質が高品質であるほどサウンドバンクのサイズは大きくなります。
4～6くらいがよいでしょう。  
![Press Edit then to define the desired quality by dragging the slider](/resources/image_20171105_09.png)

9. [**Events**] タブに進みます。
ここでは置き換えたいイベントを見つける必要があります。
イベントの説明が記載されている [**Notes**] 欄に注目してください。
`wpn_huge_PC_mod event` に “play” ルールを追加し、
追加したルールのところで右クリックして [**Browse**] を選択します。
開いたダイアログの中に、必要なサウンドファイル (`wpn_mod`) を見つける必要があります。　　
![Proceed to the Events tab](/resources/image_20171105_10.png)

10. [**SoundBanks**] タブを開きます。
新規の `.bnk` ファイルを [**Default Work Unit**] 内に作成します
(この例では `mod` とします)。  
![Create new .bnk file](/resources/image_20171105_11.png)

11. F7 を押して [**SoundBank Manager**] に進みます。
イベント (`wpn_huge_PC_mod`) を [**Hierarchy Inclusion**] 欄にドラッグします
(注: [**Events**], [**Structures**], [**Media**] のチェックがオンになっている必要があります)。  
![Drag the event](/resources/image_20171105_12.png)  
`Platforms` と `Languages` グループの必要なチェックボックスを全て有効にします。

12. [**User Settings**] に進みます。
[**Override Project SoundBank Paths**] チェックボックスを有効にし、
生成する `.bnk` のパスを定義します。
![Proceed to the User Settings](/resources/image_20171105_13.png)

13. `.bnk` が生成されたら [**Close**] をクリックします。  
![Once the .bnk generated](/resources/image_20171105_14.png)

14. フォルダ `<game_folder>/res_mods/<game_version>/audioww/` を作成し、
生成された `mod.bnk` と、プロジェクトと同じ場所に提供されている `audio_mods.xml` をコピーします。

15. `audio_mods.xml` を開き、作成した `mod.bnk` を指定する設定をします。  
![Open audio_mods.xml and specify there our mod.bnk](/resources/image_20171105_15.png)  
デフォルトイベント `obj_bicycle` を追加します。
このイベントは `obj_bicycle_mod` で置き換えられるイベントです。  
![Add default event obj_bicycle and event](/resources/image_20171105_16.png)  
(`SWITCHES`, `STATES`, `RTPC` を使用している場合は、関連する文字列に追加されていることを確認してください)  
![Add default event obj_bicycle and event](/resources/image_20171105_17.png)  
![Add default event obj_bicycle and event](/resources/image_20171105_18.png)
![Add default event obj_bicycle and event](/resources/image_20171105_19.png)  
以上で `audio_mods.xml` を保存します。

16. mod の準備ができました☺。
