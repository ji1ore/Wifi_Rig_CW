━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Wifi_Rig_CW for M5ATOM Lite  Ver1.40
Wifi リモート CW キーヤー / USB CW 中継
JI1ORE  2026/5/17
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━


【概要】
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
M5ATOM Lite を使った CW キーヤーシステムです。
2通りの使い方ができます。

  ① Wifi スタンドアロンモード
     M5ATOM Lite 2台（クライアント + サーバー）を使い、
     電鍵の ON/OFF を WiFi 経由でリアルタイムに中継します。
     有線では届かない距離（別室・屋外）の無線機をリモートキーイングできます。

  ② USB 中継モード（Android アプリ Wifi_RIG_CTRL v1.40 との組み合わせ）
     クライアント M5ATOM を Android スマートフォンに USB 接続し、
     Android アプリが Raspberry Pi 経由で無線機に CW 信号を中継します。
     CW モードではキー状態をそのまま中継、FM 等の音声モードでは
     CW 音声トーンをストリーミングして変調できます。
     外出先から VPN 越しの CW 運用も可能です。


【ファームウェアの種類】
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  Clie_M5AtomLitev1.40.bin  クライアント用（電鍵を接続する側）
  Serv_M5AtomLitev1.40.bin  サーバー用（無線機のキー端子に接続する側）

  ※ USB 中継モードのみ使用する場合はクライアントのみでも動作しますが、
     Wifi スタンドアロンモードにはクライアントとサーバーの両方が必要です。


【必要なハードウェア】
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
共通
  M5ATOM Lite           2台（クライアント用・サーバー用）
  ATOMICプロキット      2台
  MicroSD カード        2枚（WiFi プロファイル書き込み用）

クライアント側
  フォトカプラ H11L1M   1
  丸ピンICソケット(6P)  1
  ステレオジャック MJ-8435  1
  抵抗 100Ω            1
  抵抗 200Ω            1
  フェライトビーズ      3

サーバー側
  フォトカプラ PC817    1
  丸ピンICソケット(4P)  1
  ステレオジャック MJ-8435  1
  抵抗 100Ω            1

USB 中継モードを使う場合
  OTG 対応 USB ケーブル（USB-C ↔ Android スマートフォン）
  Android スマートフォン（Android 5.0 以上）
  Raspberry Pi Zero 2W（Wifi_RIG_CTRL セットアップ済み）

※ 配線図は配線フォルダの JPG ファイルを参照してください。


【モードの切り替え方】
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
クライアントファームウェアには WiFi モードと USB モードが内蔵されています。
起動後 10 秒以内にボタン操作でモードを切り替えます。

  LED 表示:
    赤     = USB 中継モード
    色付き = WiFi スタンドアロンモード（プロファイルの色）

  操作:
    短押し      起動後 10 秒以内 → WiFi プロファイルの切り替え
    長押し 1秒  USB / WiFi モードの切り替え（設定は EEPROM に保存）

  → 電源を入れるたびに前回のモード設定が引き継がれます。


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
① WiFi スタンドアロンモードのセットアップ
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

M5Burner でクライアントとサーバーにそれぞれバイナリを書き込んだ後、
WiFi プロファイルファイルを MicroSD カード経由で設定します。

【WiFi プロファイルの設定】
ソースコードフォルダ内のプロファイルテンプレートをテキストエディタで編集し、
MicroSD カードのルートに置いて M5ATOM の TF-CARD スロットに挿入します。

  クライアント用: wifi_client_profile.txt
  サーバー用    : wifi_server_profile.txt

必須項目:
  SSID        自宅 WiFi の SSID
  PASS        WiFi パスワード
  HASIP       DHCP（IPアドレスを自動取得）または IP（固定IP）
  MODE        STA（ステーション=通常モード）
  SERVERHOSTNAME  サーバーのホスト名（クライアント側のみ）

設定例（DHCP モード）:
  [Profile]
  SSID=MyHomeNetwork
  PASS=mypassword
  HOSTNAME=RKCLIENT01
  HASIP=DHCP
  MODE=STA
  COLOR=Cyan
  SERVERHOSTNAME=RKSERVER01

【起動後の LED 表示（WiFi モード）】
  プロファイル色で点灯    WiFi 接続待機中 / サーバー探索中
  緑                      サーバーと接続済み・キー OFF
  プロファイル色（明滅）  サーバーと同期中
  キー ON 中              緑色で点灯


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
② USB 中継モードのセットアップ
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

【前提】
  Android スマートフォンに Wifi_RIG_CTRL for Android v1.40 をインストール済み
  Raspberry Pi Zero 2W のセットアップ完了済み
  ※ Wifi_RIG_CTRL の詳細は GitHub の M5CoreHamCAT リポジトリを参照してください。

【接続手順】
  1. クライアント M5ATOM Lite に電鍵を接続（GROVE 端子）
  2. OTG ケーブルで M5ATOM と Android を接続
  3. M5ATOM 起動後、ボタン長押し 1秒 → LED が赤 = USB モード
  4. Android に「USB デバイスへのアクセスを許可しますか？」と表示 → OK
  5. Wifi_RIG_CTRL アプリのメイン画面に「CW relay」または「Audio relay」と表示される

【USB モード時の LED 表示（クライアント）】
  青（薄暗い）     USB モード待機中
  青（明るい）     キー ON 中

【アプリ側の動作モード】
  無線機が CW モードの場合
    → キーの ON/OFF を Raspberry Pi 経由でサーバー M5ATOM の GPIO に中継
       サーバー M5ATOM は無線機のキー端子に接続（フォトカプラ経由）

  無線機が FM/SSB 等の音声モードの場合
    → キーの ON/OFF に応じて 700Hz の CW 音声トーンを生成し
       Raspberry Pi 経由で無線機の音声入力にストリーミング

  ※ USB モードでは M5ATOM サーバーとの WiFi 接続は不要ですが、
     CW モード（GPIO 中継）を使う場合はサーバー M5ATOM が必要です。

【VPN 越しの運用（外出先から）】
  WireGuard VPN を使って外出先から Raspberry Pi に接続できます。
  遅延が発生するため、アプリの CW USB バー長押しで以下を設定:
    CW VPN buffer   : 測定値 + 200ms（推奨値はアプリが自動計算）
    FM-CW PTT delay : 150〜300ms（VPN 遅延に応じて調整）


【組み合わせシステム構成図】
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  WiFi スタンドアロンモード:

    [電鍵] → [M5ATOM Lite クライアント] ─── WiFi ───> [M5ATOM Lite サーバー] → [無線機キー端子]


  USB 中継モード（CW モード）:

    [電鍵] → [M5ATOM Lite] ─USB─> [Android] ─── WiFi/VPN ───> [Raspberry Pi] ─── WiFi ───> [M5ATOM Lite サーバー] → [無線機キー端子]

  USB 中継モード（FM-CW / 音声モード）:

    [電鍵] → [M5ATOM Lite] ─USB─> [Android] ─── WiFi/VPN ───> [Raspberry Pi] ─USB音声──> [無線機マイク入力]


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
English Description
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Overview
--------
Wifi_Rig_CW is a CW keyer system using M5ATOM Lite that supports two operation modes:

  Mode 1: WiFi Standalone
    Two M5ATOM Lite units (Client + Server) relay key ON/OFF over WiFi in real time.
    Enables remote CW keying of a radio in another room or location.

  Mode 2: USB Relay (with Android app "Wifi_RIG_CTRL" v1.40)
    Connect the Client M5ATOM to an Android smartphone via USB.
    The Android app relays key signals to a Raspberry Pi, which then controls the radio.
    - CW mode    : Key state is relayed as GPIO via Server M5ATOM → radio key terminal
    - FM/SSB etc : A 700 Hz CW audio tone is streamed to the radio's audio input
    Remote operation over WireGuard VPN (from outside your home network) is also supported.


Firmware Files
--------------
  Clie_M5AtomLitev1.40.bin   Client firmware (connect your key to this unit)
  Serv_M5AtomLitev1.40.bin   Server firmware (connect to radio key terminal via optocoupler)


Switching Between USB and WiFi Modes
-------------------------------------
Within the first 10 seconds after power-on:
  Short press   Switch WiFi profile (LED color changes per profile)
  Long press (1 sec)   Toggle between USB mode (LED = RED) and WiFi mode (LED = profile color)

The selected mode is saved to EEPROM and remembered after power cycling.


Setup — WiFi Standalone Mode
-----------------------------
1. Flash Client firmware to one M5ATOM Lite, Server firmware to the other.
2. Edit the WiFi profile template (wifi_client_profile.txt / wifi_server_profile.txt)
   with your SSID, password, and hostnames.
3. Copy the profile file to the root of a MicroSD card.
4. Insert the MicroSD into the M5ATOM's TF-CARD slot and power on.
5. The M5ATOM reads the profile and connects to WiFi automatically.

LED status (WiFi mode):
  Profile color   Connecting / searching for server
  Green           Connected to server, key OFF
  Key pressed     Green (bright)


Setup — USB Relay Mode
-----------------------
Requirements:
  Android smartphone with Wifi_RIG_CTRL v1.40 installed
  Raspberry Pi Zero 2W (set up with create_api.sh from RaspberryPiSetup folder)

Steps:
  1. Connect the key to the Client M5ATOM (GROVE terminal)
  2. Connect M5ATOM to Android via OTG USB cable
  3. Long press the M5ATOM button for 1 second until LED turns RED (USB mode)
  4. Tap "OK" when Android asks for USB device permission
  5. The Wifi_RIG_CTRL app shows "CW relay" or "Audio relay" on the main screen

LED status (USB mode):
  Dim blue   USB mode, waiting / key OFF
  Bright blue   Key ON

For CW mode (GPIO relay), the Server M5ATOM is also required.
It must be connected to the radio's key terminal via optocoupler circuit.
For FM-CW / audio streaming mode, only the Client M5ATOM is needed.


System Diagrams
---------------
WiFi Standalone:
  [Key] → [M5ATOM Client] ─── WiFi ───> [M5ATOM Server] → [Radio key terminal]

USB Relay (CW mode):
  [Key] → [M5ATOM] ─USB─> [Android] ─── WiFi/VPN ───> [Raspberry Pi] ─── WiFi ───> [M5ATOM Server] → [Radio key terminal]

USB Relay (FM-CW / audio mode):
  [Key] → [M5ATOM] ─USB─> [Android] ─── WiFi/VPN ───> [Raspberry Pi] ─── USB audio ──> [Radio mic input]


Notes
-----
- Wiring diagrams are in the 配線 (wiring) folder.
- Source code is available in the ソースコード/Ver1.40/ folder.
- WiFi profile templates are in ソースコード/Ver1.40/Wifiプロファイル/.
- The Server M5ATOM requires its own WiFi profile (wifi_server_profile.txt)
  with the hostname that the Client or Raspberry Pi will look up.
- Raspberry Pi setup scripts and the Android APK are available at:
  https://github.com/ji1ore/M5CoreHamCAT/tree/main/v1.40/
