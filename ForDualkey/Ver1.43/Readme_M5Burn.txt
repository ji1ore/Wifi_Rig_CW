━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Wifi_Rig_CW DualKey for M5ATOM S3 Lite  Ver1.43
デュアルパドル(イアンビック)対応 CW キーヤー クライアント
JI1ORE  2026/6/21
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━


【概要】
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
M5ATOM S3 Lite を使ったクライアント専用ファームウェアです。
通常版(ForM5StackLite)の単一電鍵クライアントから分岐し、
複式(縦振り)パドルを接続してイアンビック・キーヤー(Mode A、スクイズ運用)で
打鍵できるようにしたバージョンです。

サーバーとの通信プロトコルは通常版Ver1.43と共通のため、
本パッケージにはサーバー機(M5ATOM Lite)用ファームウェアを同梱しています。
WiFi スタンドアロンモードと USB 中継モード(Android/iOS)の両方に対応します。


【ファームウェアの種類】
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  Clie_M5S3Litev1.43.bin    クライアント用 (M5ATOM S3 Lite + パドルを接続する側)
  Serv_M5AtomLitev1.43.bin  サーバー用 (M5ATOM Lite, 無線機のキー端子に接続する側)

  ※ クライアント・サーバーとも通常版Ver1.43と同一プロトコルで通信します。
     サーバーは既に通常版Ver1.43をお持ちの場合は流用可能です。


【必要なハードウェア】
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
クライアント側
  M5ATOM S3 Lite        1台
  複式(縦振り)パドル     1台 (DIT/DAHの2接点)

サーバー側 (通常版Ver1.43と共通)
  M5ATOM Lite           1台
  ATOMICプロキット       1台
  フォトカプラ PC817     1
  丸ピンICソケット(4P)   1
  ステレオジャック MJ-8435  1
  抵抗 100Ω             1

USB 中継モードを使う場合
  Android: OTG対応USB-Cケーブル + Android スマートフォン(Android 5.0以上)
  iOS    : USB-C ケーブル(USB-NCM直結)
  BLE    : Bluetooth LE対応のAndroid スマートフォン(ケーブル不要)
  Raspberry Pi Zero 2W(Wifi_RIG_CTRL セットアップ済み、CWモードでサーバー中継する場合)

※ クライアント側の配線図は未作成です。下記ピン配置を参照してください。


【ピン配置 (M5ATOM S3 Lite クライアント)】
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  DIT_PIN  (右パドル/短点)   G0
  DAH_PIN  (左パドル/長点)   G17
  BUTTON_PIN (本体ボタン)    G41
  LED_DATA_PIN (WS2812B)    G21  ([0]=左LED/DAH, [1]=右LED/DIT)
  LED_PWR_PIN  (LED電源)     G40  (HIGH=ON)


【動作モードの切り替え方】
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
起動後10秒以内にパドル/ボタン操作でモードを切り替えます。

  LED表示 (待機ウィンドウ中):
    赤      = USB中継モード・BLE選択中
    オレンジ = USB中継モード・CDC/NCM選択中
    色付き  = WiFiスタンドアロンモード(プロファイルの色、点滅)

  操作:
    左パドル(DAH)を押す       USB中継モードに切替(CDC/NCM側、EEPROM保存)
    右パドル(DIT)を押す       USB中継モード・BLEワイヤレスに切替(EEPROM保存)
    本体ボタン 短押し          (WiFiモード時)プロファイルを切替
    本体ボタン 長押し(1秒)     WiFi ⇔ USB中継モードを切替(EEPROM保存)

  → 電源を入れるたびに前回のモード設定が引き継がれます。
  → USB中継・BLE選択時に電源投入後、アプリからUSBデータを受信すると
     自動的にUSB-CDCモードへ切り替わります(充電器接続等との誤判定防止)。


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
① WiFi スタンドアロンモード
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
本バージョンはWiFiプロファイルをEEPROMで管理し、SDカードは使用しません。
初回起動時はソースコード内の fallbackProfile (SSID: RemoteKeyer-AP) でUSBモード起動します。
WiFiプロファイルを変更する場合は、ソースコード上の wifiProfiles[] / fallbackProfile を
書き換えて再ビルドし、書き込んでください。

【起動後のLED表示 (WiFiモード)】
  プロファイル色で点灯(点滅)   サーバー探索中・同期待ち
  緑                          サーバーと同期済み・キーOFF
  キーON中                    緑色で点灯


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
② USB 中継モード (Wifi_RIG_CTRL for Android/iOS との組み合わせ)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

本バージョンは3つの中継経路を切り替えて使用できます。

  ・USB-CDC  : Android とUSBケーブルで接続(シリアルトンネル)
  ・USB-NCM  : iOS とUSB-Cで直結(192.168.7.x のIPネットワーク, USBNCM.h対応ビルド時のみ)
  ・BLE NUS  : Android とケーブルなしでBluetooth LE接続(Nordic UART Service)
               デバイス名: "DualKey-BLE"

  ※ USBNCM.h非対応のビルド環境では自動的にUSB-CDCシリアルトンネルにフォールバックします。

【LED表示 (USB-CDCモード/クライアント)】
  黄色点滅(500ms)    アプリ接続済み・時刻同期待ち
  オレンジ点滅(200ms) 時刻同期パケット受信中
  右LED赤点滅(1000ms) アプリ未接続
  緑                 時刻同期完了・使用可能
  キーON中            左=DAH(青)/右=DIT(緑)で点灯

【LED表示 (BLEモード/クライアント)】
  青点滅(1000ms)  BLEクライアント未接続
  黄色点滅(500ms) BLE接続済み・時刻同期待ち
  緑              時刻同期完了・使用可能
  キーON中         青で点灯

【アプリ側の動作モード】
  無線機が CW モードの場合
    → キーのON/OFFをRaspberry Pi経由でサーバーM5ATOMのGPIOに中継
       サーバーM5ATOMは無線機のキー端子に接続(フォトカプラ経由)

  無線機がFM/SSB等の音声モードの場合
    → キーのON/OFFに応じて700Hzの CW音声トーンを生成しRaspberry Pi経由でストリーミング

  ※ BLEモードはアプリとM5ATOM間のみケーブル不要になるもので、
     Raspberry Pi以降の中継構成は他モードと同一です。


【イアンビック・キーヤー (Mode A)】
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
複式パドルでスクイズ運用ができます。
  DIT単独    : 短点を連続送出
  DAH単独    : 長点を連続送出
  スクイズ   : DIT/DAHを交互に自動送出(Mode A、ダブルスクイズ無し)

  WPM       : デフォルト20。アプリから送信される0xE2パケットで5〜99の範囲で
              即時変更可能(EEPROMに保存され次回起動時も保持)
  パドル入替 : アプリから送信される0xE3パケットで左右入替可能
              (EEPROMに保存され次回起動時も保持)
  安全タイムアウト: キーON状態が3秒(MAX_ON_DURATION)続くと強制的にOFFします。


【組み合わせシステム構成図】
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  WiFiスタンドアロンモード:

    [パドル] → [M5ATOM S3 Lite クライアント] ─WiFi─> [M5ATOM Lite サーバー] → [無線機キー端子]


  USB中継モード(CDC/NCM, CWモード):

    [パドル] → [M5ATOM S3 Lite] ─USB(CDC/NCM)─> [Android/iOS] ─WiFi/VPN─> [Raspberry Pi] ─WiFi─> [M5ATOM Lite サーバー] → [無線機キー端子]


  BLE中継モード(CWモード):

    [パドル] → [M5ATOM S3 Lite] ─BLE(NUS)─> [Android] ─WiFi/VPN─> [Raspberry Pi] ─WiFi─> [M5ATOM Lite サーバー] → [無線機キー端子]


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
English Description
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Overview
--------
This is a client-only firmware for M5ATOM S3 Lite, forked from the standard
single-key client (ForM5StackLite). It adds support for a dual-lever paddle,
driving a built-in Iambic Mode A (squeeze) keyer.

The network protocol is identical to the standard Ver1.43 client, so the
bundled Server firmware (M5ATOM Lite) is the same one shipped with the
standard release and can be reused if you already have it.

Firmware Files
--------------
  Clie_M5S3Litev1.43.bin    Client (M5ATOM S3 Lite, connect a paddle to this unit)
  Serv_M5AtomLitev1.43.bin  Server (M5ATOM Lite, connect to radio key terminal via optocoupler)

Pin Assignment (M5ATOM S3 Lite client)
---------------------------------------
  DIT_PIN  (dit/right paddle)   G0
  DAH_PIN  (dah/left paddle)    G17
  BUTTON_PIN                    G41
  LED_DATA_PIN (WS2812B)        G21  ([0]=left/DAH, [1]=right/DIT)
  LED_PWR_PIN                   G40  (HIGH=ON)

Switching Between Modes
------------------------
Within the first 10 seconds after power-on:
  Press DAH (left) paddle     Switch to USB relay mode, CDC/NCM side
  Press DIT (right) paddle    Switch to USB relay mode, BLE wireless side
  Short press main button     (WiFi mode) cycle WiFi profile
  Long press main button (1s) Toggle between WiFi mode and USB relay mode

The selected mode is saved to EEPROM and remembered after power cycling.
If BLE mode is selected at boot but USB serial data arrives from the app,
the client automatically falls back to USB-CDC mode (avoids misdetecting a
charger as a host).

Setup - WiFi Standalone Mode
------------------------------
This version stores WiFi profiles in EEPROM; no MicroSD card is used.
On first boot it starts in USB mode using the built-in fallback profile
(SSID: RemoteKeyer-AP). To use your own WiFi network, edit wifiProfiles[]
/ fallbackProfile in the source code and reflash.

Setup - USB Relay Mode
------------------------
Three relay paths are supported:
  USB-CDC : wired USB connection to Android (serial tunnel)
  USB-NCM : direct USB-C connection to iOS (192.168.7.x IP network,
            requires a build with USBNCM.h support; otherwise falls back
            to USB-CDC automatically)
  BLE NUS : cable-free Bluetooth LE connection to Android
            (Nordic UART Service, device name "DualKey-BLE")

Requirements:
  Android smartphone with Wifi_RIG_CTRL for Android v1.43 installed
  Raspberry Pi Zero 2W (set up with Wifi_RIG_CTRL), required for CW mode relay

Iambic Keyer (Mode A)
-----------------------
  DIT only    : repeated dits
  DAH only    : repeated dahs
  Squeeze     : alternating dit/dah (Mode A, no dual-squeeze memory)

  WPM         : default 20. Settable 5-99 at runtime via a 0xE2 packet sent
                from the app (CDC/BLE); saved to EEPROM and restored on boot.
  Paddle swap : settable via a 0xE3 packet sent from the app; saved to
                EEPROM and restored on boot.
  Safety cutoff: key is forced OFF if held ON for more than 3 seconds
                 (MAX_ON_DURATION).

System Diagrams
---------------
WiFi Standalone:
  [Paddle] -> [M5ATOM S3 Lite Client] -- WiFi --> [M5ATOM Lite Server] -> [Radio key terminal]

USB Relay (CDC/NCM, CW mode):
  [Paddle] -> [M5ATOM S3 Lite] --USB(CDC/NCM)--> [Android/iOS] -- WiFi/VPN --> [Raspberry Pi] -- WiFi --> [M5ATOM Lite Server] -> [Radio key terminal]

BLE Relay (CW mode):
  [Paddle] -> [M5ATOM S3 Lite] --BLE(NUS)--> [Android] -- WiFi/VPN --> [Raspberry Pi] -- WiFi --> [M5ATOM Lite Server] -> [Radio key terminal]

Notes
-----
- This client differs from the standard Ver1.43 client by adding: dual-paddle
  Iambic keying, BLE wireless relay mode, paddle swap, and runtime WPM control.
  Everything else (network protocol, Server firmware) is shared with the
  standard release.
- Source code is archived in ソースコード/Ver1.43_DualKey/.
- No wiring diagram is available yet for the paddle connection; refer to the
  pin assignment above.
