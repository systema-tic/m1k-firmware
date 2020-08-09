# m1k-firmware

[Zaunkoenig M1K](https://zaunkoenig.co/)用のオープンソースファームウェアです。以下では、本ファームウェアを Linux および Windows で M1K にインストールする方法を説明します。

## WindowsでZaunkoenig M1K firmwareをインストールする
ファームウェアのバイナリファイルをすでに入手している場合 (誰かが送ってくれた場合など) は、直接ステップ 10 に進むことができます。
そうでない場合は、まずGitHubからファームウェアファイルをダウンロードする必要があります。
1. https://github.com/zaunkoenig-firmware/m1k-firmware から M1K ファームウェアをダウンロードして解凍してください。(ファイルリストの右上隅にある緑色の "clone/Download "ボタンをクリック)
2. Windowsのスタートメニューから「Microsoft Store」を開きます。
3. 検索バーに「WSL」と入力します。
4. 「Ubuntu」をクリックし、「入手」をクリックします。
5. ダウンロードが完了したら「起動」をクリックします。
    - 以下のようなエラーが表示された場合はWSLを有効化して下さい。
  (参考： [WSLの有効化](http://www.aise.ics.saitama-u.ac.jp/~gotoh/HowToEnableWSL.html))
  ```
  Installing, this may take a few minutes...
  WslRegisterDistribution failed with error: 0x8007019e
  The Windows Subsystem for Linux optional component is not enabled. Please enable it and try again.
  See https://aka.ms/wslinstall for details.
  Press any key to continue...
  ```
6. ターミナルが開き、インストールプロセスが表示されます。ユーザー名とパスワードを選択して、インストールが完了するのを待つだけです（数分で完了します）。
7. 手順6の同じターミナルで、M1Kファームウェアがダウンロードされたフォルダを開く必要があります。開きたいフォルダが次の場所に保存されているとするとします。`C:\Users\<<ユーザ名>>\Downloads\m1k-firmware-master\m1k-firmware-master` その場合、ターミナルに次のコマンドを入力すると、フォルダが開きます。`cd /mnt/c/Users/<<ユーザ名>>/Downloads/m1k-firmware-master/m1k-firmware-master`
8. 以下の3つのコマンドを実行します:  
`sudo apt-get update -qq`  
`sudo apt-get install -qq gcc-avr binutils-avr avr-libc`  
`make`
9. サイドのmakeを実行すると`C:\Users\<<ユーザ名>>\Downloads\m1k-firmware-master\m1k-firmware-master`にtwobtn.hexとtwobtn.eepという2つのファームウェアファイルが出力されます。
10. マウスへファームウェアを書き込むプログラムをインストールします。Atmel USBマイクロコントローラ用のコマンドラインプログラマ[Atmel USB DFU Programmer](https://sourceforge.net/projects/dfu-programmer/)をダウンロードして解凍ください(以下の手順ではダウンロードフォルダに解凍したものとして進めます)。
11. DFU Programmer をインストールするには、解凍したファイル内の"dfu-prog-usb-1.2.2"フォルダを開き、atmel_usb_dfu.inf を右クリックして "インストール"をクリックしてください。
12. インストールしたい 2 つのファームウェアファイルを dfu-programmer.exeのあるフォルダにコピーします。1 つ目のファームウェアファイルは twobtn.hex で、2 つ目のファームウェアファイルは twobtn.eep です。
13. ここで、M1Kを「ファームウェアアップデートモード」にする必要があります。これには2つの方法があります。1つ目は、左スイッチの右側にある2つの金色のピンをショートさせる方法です。しかし、この方法ではマウスを分解する必要があります。もう一つの方法は、M1Kを分解しない方法で、マウスの両ボタンを押しながらM1Kを接続します。5秒後にカーソルが時計回りに四角く移動し、Angle SnappingとLODがデフォルト値に変更されたことを示します。しかし、今回はまだマウスボタンを離さないでください。マウスカーソルがフリーズするまで、さらに 5 秒間押し続けてください。
14. Windowsのコマンドプロンプトを起動(Windowsキー→`cmd`と入力→Enter)し、以下を実行してdfu-programmer.exeのあるフォルダに移動します。: `cd C:\Users\<<ユーザ名>>\Downloads\dfu-programmer-win-0.7.2`
    - Ubuntuのターミナル画面ではなく、通常のコマンドプロンプトであることに注意してください。
15. 以下の4つのコマンドを実行します(各コマンドは終了するまでに数秒かかります)。 :  
`dfu-programmer atmega32u2 erase --force`  
`dfu-programmer atmega32u2 flash twobtn.hex`  
`dfu-programmer atmega32u2 flash-eeprom twobtn.eep --force`  
`dfu-programmer atmega32u2 start`
16. サイドのstartコマンドを実行した後は、マウスは新しいファームウェアで起動して動作しているはずです。

## Installing the firmware files on the M1K in Linux (翻訳中)
When you already have the firmware files (because someone sent them to you for example) you can jump directly to step 4. Else you have to download the firmware files from GitHub first:
1. Download and unzip the M1K firmware at https://github.com/zaunkoenig-firmware/m1k-firmware by clicking on the green «clone/download» button at the right top corner of the files list.
2. Open up a terminal and execute the following three commands:  
`sudo apt-get update -qq`  
`sudo apt-get install -qq gcc-avr binutils-avr avr-libc`  
`make -C /home/Patrick/Downloads/m1k-firmware-master\m1k-firmware-master`
3. After make has been executed the two firmware files have appeared in C:\Users\Patrick\Downloads\m1k-firmware-master\m1k-firmware-master. The .hex file is called twobtn.hex and the .eep file is called twobtn.eep.
4. Install DFU programmer by executing the following two commands in a terminal:  
`sudo apt-get update -y`  
`sudo apt-get install -y dfu-programmer`
5. Copy the two firmware files you want to install into the DFU Programmer folder. The first firmware file is called twobtn.hex and the second one is called twobtn.eep.
6. Now you have to put the M1K into the «firmware update mode». There are two ways to do that. The first way is to short-circuit the two golden pins to the right side of the left switch: you know you succeeded when the mouse cursor freezes. However, for this method you have to open your mouse. The other way is possible without having to open up your M1K:  
Plug in the M1K while holding down both mouse buttons. After five seconds the cursor will move clockwise in a square to indicate that Angle Snapping and LOD have been changed to default values. This time though, do not let go of your mouse buttons yet. Keep them pressed down for five more seconds until your mouse cursor has frozen.
7. Now run the following four commands (each command will take a few seconds to finish):  
`sudo dfu-programmer atmega32u2 erase --force`  
`sudo dfu-programmer atmega32u2 flash twobtn.hex`  
`sudo dfu-programmer atmega32u2 flash-eeprom twobtn.eep --force`  
`sudo dfu-programmer atmega32u2 start`
8. After you executed the start command your mouse should be up and running with the new firmware.
