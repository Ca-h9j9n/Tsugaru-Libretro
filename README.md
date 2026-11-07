# Tsugaru Libretro

A Libretro port of Tsugaru, the FM TOWNS emulator.

## English

### 1. Overview

Tsugaru Libretro allows FM TOWNS software to run in RetroArch.

Supported platforms:

- Linux x86_64
- Linux ARM64 (aarch64)
- Android ARM64 (arm64-v8a)
- Windows x86_64

### 2. Installation

| Platform | Core file |
| --- | --- |
| Linux x86_64 | tsugaru_libretro.so |
| Linux ARM64 | tsugaru_libretro.so |
| Android ARM64 | tsugaru_libretro.so |
| Windows x86_64 | tsugaru_libretro.dll |

The release also includes tsugaru_libretro.info.

Check RetroArch Settings > Directory for the Cores
and Core Info directory locations.

Install the core binary and its matching .info file.

On the tested Linux AM/AppImage installation, both
files are stored in the portable RetroArch cores folder.

System-installed RetroArch may use protected directories.
Back up existing cores before replacing them.

#### Linux x86_64 / Windows x86_64 Installation

1. Open RetroArch Settings > Directory.
2. Check the Cores and Core Info directories.
3. Copy tsugaru_libretro.so (Linux) or
   tsugaru_libretro.dll (Windows) to Cores.
4. Copy tsugaru_libretro.info to Core Info.
5. Restart RetroArch and load Tsugaru.

On some Linux installations, Cores and Core Info
may point to the same directory.

System-installed Linux packages may require
administrator privileges for file copying.

Back up existing Tsugaru files before replacing them.

#### Android ARM64 Installation

1. Create /storage/emulated/0/RetroArch/info.
2. In RetroArch, open Settings > Directory >
   Core Info and select the new folder.
3. Run Online Updater > Update Core Info Files.
4. Copy tsugaru_libretro.info into the new folder.
5. Use Install or Restore a Core to install
   the Android ARM64 tsugaru_libretro.so.
6. Restart RetroArch and check that Tsugaru
   appears in Load Core with version 1.0.0.

This method was verified without root or ADB.
Other installed cores remained listed.

Keep a record of the original Core Info path
in case you need to restore the previous setting.

#### Linux ARM64 Installation (ROCKNIX)

On ROCKNIX, installation is straightforward.

1. Copy tsugaru_libretro.so and
   tsugaru_libretro.info to the game ROM microSD card.
2. Boot ROCKNIX.
3. Open Tools > File Manager.
4. Copy both files to /storage/cores.
5. Load the Tsugaru core in RetroArch.

This procedure was verified on ROCKNIX.
On devices with a different storage configuration,
the initial file transfer method may differ.

Other Linux ARM64 firmware may use different
user-accessible core directories.

KNULLI and Batocera are not covered by
these installation instructions.

### 3. FM TOWNS ROM Setup

FM TOWNS system ROM files are required.

Create a folder named fmtowns inside RetroArch's
configured System/BIOS directory.

Required ROM files:

- FMT_SYS.ROM
- FMT_DOS.ROM
- FMT_FNT.ROM
- FMT_DIC.ROM

FMT_F20.ROM is optional.
FMT_ALL.ROM is also supported as an alternative ROM layout.

ROM files are not included in this release.

For ROM preparation, refer to the original Tsugaru project:
https://github.com/captainys/TOWNSEMU

### 4. Supported Content

Recognized file extensions:

cue, iso, mds, bin, chd, d77, d88, hdm, h0, icm, tsu

Compatibility with every game or disk image is not guaranteed.

### 5. TSU Configuration Files

A .tsu file can specify disks and memory configuration.

Example:

    -MEMSIZE 6M
    -FD0 "disk1.hdm"
    -FD1 "disk2.d88"
    -FD1NEXT "disk3.d88"
    -CD "disc1.cue"
    -CDNEXT "disc2.cue"
    -HD0 "game.h0"
    -ICM "game.icm"

Write one option per line.
Use uppercase option names.

Relative image paths are resolved from the .tsu directory.

-MEMSIZE 6M selects 6 MB of emulated RAM.
This notation is compatible with Tsugaru CUI.

Only specify the media required for your game.

### 6. Disk Swapping

The core supports RetroArch Disk Control.

For multiple CD images, use:

    -CD "disc1.cue"
    -CDNEXT "disc2.cue"

For multiple FD1 images, use:

    -FD1 "disk1.hdm"
    -FD1NEXT "disk2.hdm"

Open Quick Menu > Disk Control to change disks.

When multiple CDs are registered, CD swapping takes
priority. Otherwise, FD1 swapping is available.

Some RetroArch versions do not display the current
disk index, even when disk swapping works.

When both multiple CDs and multiple FD1 disks are
registered in the same TSU file, RetroArch Disk
Control handles CD swapping only.

FD1 swapping cannot be selected separately while
multiple CD images are registered.

If only one CD image is registered, FD1 swapping
remains available when FD1 disk images are listed.

### 7. Controller Configuration

The core supports three input device types:

- FM TOWNS 2 Button Pad
- FM TOWNS 6 Button Pad
- FM TOWNS Mouse

Select the device in RetroArch:

Quick Menu > Controls > Port 1 Controls > Device Type

Six-button games may also require in-game setup.

Mouse operation is normally available even when
FM TOWNS 2 Button Pad is selected.

If the mouse does not work in RetroArch on Windows,
open Settings > Input > RetroPad Binds >
Port 1 Controls and set Mouse Index to Default.
There is no need to change Device Index.

The FM TOWNS Mouse device type is primarily intended
for Linux ARM64 handhelds without touchscreen input.

RetroArch menu shortcuts depend on the platform.

Examples from tested environments:

- Linux x86_64 / Windows: F1
- Android: on-screen RetroArch menu icon
- ROCKNIX: SELECT + DOWN

These shortcuts may differ on other installations.

### 8. CMOS and Storage Persistence

The core stores FM TOWNS CMOS data at:

    RetroArch System Directory/fmtowns/cmos.dat

The System/BIOS directory is configured in RetroArch.

CMOS persistence is supported for both direct media
launches and TSU-based launches.

The core also supports writing changes to supported
HDD (H0) and ICM disk images.

Important notes:

- Keep backups of CMOS and writable disk images.
- Ensure the relevant directories are writable.
- Avoid removing media while data is being written.
- Exit RetroArch normally whenever possible.

Storage behavior:

- CMOS is saved when the core unloads normally.
- Modified floppy disk images are saved periodically
  and when the core unloads.
- ICM images are saved periodically when modified.
  There is no explicit ICM save call on core unload.
- HDD (H0) images are updated during SCSI write
  operations rather than by a periodic save routine.

CMOS uses the RetroArch System Directory shown above.
Writable disk images use their configured file paths.

Avoid force-closing RetroArch or powering off the
device immediately after writing data, especially
when using ICM images.

### 9. Known Limitations

- Save states are not supported.
- Disk Control menus vary by RetroArch version.
- Not all FM TOWNS games are guaranteed to work.
- Some installation procedures require verification.


### 10. Credits and Licenses

Original Tsugaru emulator:
CaptainYS (Soji Yamakawa)

Libretro port and modifications:
Ca-h9j9n

Core license metadata: BSD-3-Clause.

See the included license files for applicable
third-party licenses and copyright notices.

Original Tsugaru project:
https://github.com/captainys/TOWNSEMU

---

## 日本語

### 1. 概要

Tsugaru Libretroは、FM TOWNSエミュレーター
「Tsugaru」をRetroArchで動作させるための
Libretroコアです。

今回のリリースでは、以下の4種類を提供します。

- Linux x86_64
- Linux ARM64（aarch64）
- Android ARM64（arm64-v8a）
- Windows x86_64

利用にはRetroArchと、適切に用意した
FM TOWNSのシステムROMが必要です。

### 2. インストール

使用するOSに対応したコアを導入してください。

| OS | コアファイル |
| --- | --- |
| Linux x86_64 | tsugaru_libretro.so |
| Linux ARM64 | tsugaru_libretro.so |
| Android ARM64 | tsugaru_libretro.so |
| Windows x86_64 | tsugaru_libretro.dll |

共通のコア情報ファイルとして
tsugaru_libretro.info も使用します。

RetroArchの「設定 > ディレクトリ」で、
コアとコア情報の保存先を確認してください。

検証したLinux AM/AppImage版では、
コアと.infoを同じフォルダに配置しています。

システム版RetroArchでは、管理者権限が
必要なディレクトリを使用する場合があります。

既存のコアや設定は必ずバックアップしてください。

#### Linux x86_64・Windows x86_64版のインストール

1. RetroArchの「設定 > ディレクトリ」を開きます。
2. 「コア」と「コア情報」の保存先を確認します。
3. Linuxではtsugaru_libretro.so、
   Windowsではtsugaru_libretro.dllを
   コアの保存先にコピーします。
4. tsugaru_libretro.infoを
   コア情報の保存先にコピーします。
5. RetroArchを再起動し、Tsugaruを選択します。

Linuxでは、コアとコア情報の保存先が
同じディレクトリの場合もあります。

システムパッケージ版Linuxでは、
コピーに管理者権限が必要な場合があります。

既存のTsugaruファイルを上書きする場合は、
事前にバックアップしてください。

#### Android ARM64版のインストール

1. 内部ストレージに次のフォルダを作成します。
   /storage/emulated/0/RetroArch/info
2. RetroArchの「設定 > ディレクトリ >
   コア情報」で作成したフォルダを指定します。
3. 「オンラインアップデータ >
   コア情報ファイルを更新」を実行します。
4. tsugaru_libretro.infoを作成した
   infoフォルダにコピーします。
5. 「コアのインストールまたは復元」から
   Android ARM64版tsugaru_libretro.soを
   インストールします。
6. RetroArchを再起動し、「コアをロード」で
   Tsugaru（バージョン1.0.0）を確認します。

この方法はroot権限やADBを使用せずに
実機で導入できることを確認しています。
既存のコアも引き続き一覧に表示されました。

設定を元に戻せるよう、変更前の
コア情報ディレクトリを控えてください。

#### Linux ARM64版のインストール（ROCKNIX）

ROCKNIXでは以下の手順で導入できます。

1. tsugaru_libretro.soと
   tsugaru_libretro.infoをゲームROM用の
   microSDカードにコピーします。
2. ROCKNIXを起動します。
3. Tools > File Managerを開きます。
4. 2つのファイルを/storage/coresにコピーします。
5. RetroArchでTsugaruコアを使用します。

この方法はROCKNIXで動作確認済みです。
microSDカードの構成によっては、
最初のファイル転送方法が異なります。

その他のLinux ARM64向けCFWでは、
コアの保存先が異なる場合があります。

KNULLIとBatoceraは今回の
インストール手順の対象外です。

### 3. BIOS・ROMの配置

FM TOWNSのシステムROMが必要です。

RetroArchのSystem/BIOSディレクトリ内に
fmtownsフォルダを作成してください。

基本的に必要なROMファイル：

- FMT_SYS.ROM
- FMT_DOS.ROM
- FMT_FNT.ROM
- FMT_DIC.ROM

FMT_F20.ROMは任意です。
FMT_ALL.ROMを使用する構成にも対応しています。

ROMファイルは本リリースに含まれません。

ROMの準備方法については、
Tsugaru本家の説明を参照してください。

https://github.com/captainys/TOWNSEMU

### 4. 対応ファイル形式

RetroArchに登録される拡張子：

- CD：cue、iso、mds、bin、chd
- FD：d77、d88、hdm
- HDD：h0
- メモリーカード：icm
- 起動設定ファイル：tsu

すべてのゲームやディスクイメージの
動作を保証するものではありません。

### 5. TSU設定ファイル

.tsuファイルを使用すると、ディスクや
メモリ容量などの起動設定をまとめられます。

記述例：

    -MEMSIZE 6M
    -FD0 "disk1.hdm"
    -FD1 "disk2.d88"
    -FD1NEXT "disk3.d88"
    -CD "disc1.cue"
    -CDNEXT "disc2.cue"
    -HD0 "game.h0"
    -ICM "game.icm"

オプションは1行につき1つ記述します。
オプション名は大文字で指定してください。

相対パスは.tsuファイルの場所を基準にします。

-MEMSIZE 6Mはメモリ容量6MBの指定です。
Tsugaru CUIとの互換性を考慮した表記です。

ゲームに必要な項目だけを記述してください。

### 6. ディスク交換

RetroArchのディスクコントロールに対応しています。

複数CDの登録例：

    -CD "disc1.cue"
    -CDNEXT "disc2.cue"

複数FD1の登録例：

    -FD1 "disk1.hdm"
    -FD1NEXT "disk2.hdm"

交換は「クイックメニュー > ディスクコントロール」
から行います。

複数CDが登録されている場合はCD交換が優先され、
それ以外の場合はFD1交換が利用できます。

RetroArchの環境によっては、ディスク交換が
動作していても現在のディスク番号が表示されません。

同じTSUファイルにCDとFD1をそれぞれ複数枚
登録した場合、RetroArchのディスクコントロールは
CD交換のみを扱います。

CDが複数登録されている間は、
FD1交換を個別に選択することはできません。

CDの登録が1枚だけの場合は、
FD1が複数登録されていればFD1交換を利用できます。

### 7. コントローラー設定

以下の3種類の入力デバイスに対応しています。

- FM TOWNS 2 Button Pad
- FM TOWNS 6 Button Pad
- FM TOWNS Mouse

RetroArchで以下のメニューを開きます。

クイックメニュー > コントロール >
ポート1コントロール > デバイスタイプ

使用するデバイスを選択してください。

6ボタン対応ゲームでは、ゲーム側の
コントローラー設定も必要な場合があります。

通常はFM TOWNS 2 Button Padの設定でも
マウス操作が可能です。

Windows版RetroArchでマウスが動作しない場合は、
「設定 > 入力 > RetroPadバインド >
ポート1コントロール」の「マウス番号」を
「デフォルト」に設定してください。
「デバイス番号」を変更する必要はありません。

FM TOWNS Mouseへの切り替えは、主に
タッチ機能のないLinux ARM64携帯ゲーム機などで
使用するための機能です。

RetroArchメニューの呼び出し例：

- Linux x86_64 / Windows：F1
- Android：画面上のRetroArchメニューアイコン
- ROCKNIX：SELECT + DOWN

操作方法はRetroArchやファームウェアの
設定によって異なる場合があります。

### 8. CMOS・ストレージの保存

CMOSの保存先：

    RetroArchのSystemディレクトリ/fmtowns/cmos.dat

System/BIOSディレクトリはRetroArchで設定します。

.tsu起動と、.cueなどの直接起動の両方で
CMOSの保存に対応しています。

対応するHDD（H0）とICMイメージへの
書き込み保存にも対応しています。

注意事項：

- CMOSとディスクイメージはバックアップしてください。
- 保存先の書き込み権限を確認してください。
- 書き込み中のメディア交換は避けてください。
- 可能な限りRetroArchを正常終了してください。

各ストレージの保存方式：

- CMOSはコアの正常終了時に保存されます。
- FDイメージの変更内容は定期的に保存され、
  コア終了時にも保存されます。
- ICMイメージは変更があった場合に定期保存されます。
  コア終了時の明示的なICM保存処理はありません。
- HDD（H0）イメージは定期保存ではなく、
  SCSI書き込み処理中に更新されます。

CMOSの保存先は、上記のRetroArchの
Systemディレクトリに従います。
書き込み可能なディスクイメージは、
それぞれ指定されたファイルに保存されます。

データの書き込み直後にRetroArchを強制終了したり、
本体の電源を切ったりしないでください。
特にICMイメージの使用時は注意してください。

### 9. 既知の制限事項

- 今回のリリースではステートセーブに非対応です。
- ディスクコントロールのメニュー表示は
  RetroArchのバージョンや環境によって異なります。
- すべてのFM TOWNSゲームの動作を保証する
  ものではありません。
- 一部のOS別インストール手順は確認中です。


### 10. クレジット・ライセンス

Tsugaru原作者：
CaptainYS（Soji Yamakawa）

Libretro移植・改修：
Ca-h9j9n

コア情報に記載されているライセンス：
BSD-3-Clause

同梱のライセンスファイルには、
サードパーティーの著作権表示や
ライセンス情報も含まれています。

Tsugaru本家プロジェクト：
https://github.com/captainys/TOWNSEMU
