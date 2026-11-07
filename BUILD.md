# Tsugaru Libretro — Build Instructions / ビルド手順

## Overview / 概要

This document describes how to build the four platform-specific Tsugaru Libretro cores from their respective source directories. Commands are intended to be run from the directory containing `Linux_x86_64`, `Linux_ARM64`, `Android_ARM64`, and `Windows_x86_64`.

この文書は、4種類のプラットフォーム別ソースからTsugaru Libretroコアをビルドする手順です。各コマンドは、`Linux_x86_64`、`Linux_ARM64`、`Android_ARM64`、`Windows_x86_64` を含むディレクトリで実行してください。

These configurations were reconstructed from the release build's CMake caches. The commands have **not yet been independently validated as a complete clean build**. Rebuilt binaries may have different SHA-256 hashes from distributed binaries even when their executable code is equivalent.

以下の設定は公開用ビルドのCMakeキャッシュから再構成しました。**クリーンビルド手順全体の再検証は未実施**です。再ビルド後のSHA-256が配布バイナリと一致するとは限りません。

## Requirements / 必要な環境

- CMake 3.20 or newer / CMake 3.20以降
- C++17-capable compiler / C++17対応コンパイラー
- GNU Make for Linux and Windows cross-builds / LinuxおよびWindowsクロスビルド用GNU Make
- Ninja for Android / Android用Ninja
- Appropriate platform toolchains and development libraries / 対応するツールチェーンと開発ライブラリ

Build artifacts are placed in separate `build-local` directories. **Do not overwrite the distributed binaries.**

生成物は各 `build-local` ディレクトリに分離します。**配布用バイナリを上書きしないでください。**

## 1. Linux x86_64

Verified cache / 確認済みキャッシュ: GCC (`/usr/bin/cc`, `/usr/bin/c++`), Unix Makefiles, Release, `-O3 -DNDEBUG`, `CHDR_WANT_TESTS=OFF`.

```sh
cmake -S Linux_x86_64/Source -B Linux_x86_64/build-local \
  -G "Unix Makefiles" -DCMAKE_BUILD_TYPE=Release \
  -DCHDR_WANT_TESTS=OFF
cmake --build Linux_x86_64/build-local --target tsugaru_libretro -j 4
```

Expected / 生成予定: `Linux_x86_64/build-local/libretro/tsugaru_libretro.so`

## 2. Linux ARM64 (aarch64)

Verified cache / 確認済みキャッシュ: `aarch64-linux-gnu-gcc`, `aarch64-linux-gnu-g++`, Unix Makefiles, Release, `-O3 -DNDEBUG`, `CHDR_WANT_TESTS=OFF`.

```sh
cmake -S Linux_ARM64/Source -B Linux_ARM64/build-local \
  -G "Unix Makefiles" \
  -DCMAKE_SYSTEM_NAME=Linux -DCMAKE_SYSTEM_PROCESSOR=aarch64 \
  -DCMAKE_C_COMPILER=aarch64-linux-gnu-gcc \
  -DCMAKE_CXX_COMPILER=aarch64-linux-gnu-g++ \
  -DCMAKE_BUILD_TYPE=Release -DCHDR_WANT_TESTS=OFF
cmake --build Linux_ARM64/build-local --target tsugaru_libretro -j 4
```

Expected / 生成予定: `Linux_ARM64/build-local/libretro/tsugaru_libretro.so`

Target device runtime library compatibility must be checked separately.

実機のランタイムライブラリとの互換性は別途確認が必要です。

## 3. Android ARM64 (arm64-v8a)

Verified cache / 確認済みキャッシュ: Android NDK r27d, ABI `arm64-v8a`, Android API 21, Ninja, Release, `CHDR_WANT_TESTS=OFF`.

Set `ANDROID_NDK` to your local NDK r27d installation directory. / `ANDROID_NDK` をNDK r27dのインストール先に変更してください。

```sh
export ANDROID_NDK="$HOME/Android/ndk/android-ndk-r27d"
cmake -S Android_ARM64/Source -B Android_ARM64/build-local \
  -G Ninja \
  -DCMAKE_TOOLCHAIN_FILE="$ANDROID_NDK/build/cmake/android.toolchain.cmake" \
  -DANDROID_ABI=arm64-v8a -DANDROID_PLATFORM=android-21 \
  -DCMAKE_BUILD_TYPE=Release -DCHDR_WANT_TESTS=OFF
cmake --build Android_ARM64/build-local --target tsugaru_libretro -j 4
```

Expected / 生成予定: `Android_ARM64/build-local/libretro/tsugaru_libretro.so`

The inspected cache had empty `CMAKE_C_FLAGS_RELEASE` and `CMAKE_CXX_FLAGS_RELEASE` entries; no additional optimization flags are asserted here.

確認したキャッシュでは `CMAKE_C_FLAGS_RELEASE` と `CMAKE_CXX_FLAGS_RELEASE` が空欄でした。追加の最適化フラグは断定しません。

## 4. Windows x86_64 (cross-build on Linux / Linuxからのクロスビルド)

Verified cache / 確認済みキャッシュ: MinGW-w64 GCC/G++, Windows target, Unix Makefiles, Release, `CHDR_WANT_TESTS=OFF`.

**Required Windows release flags / Windows版必須フラグ:** `-O3 -fno-strict-aliasing -DNDEBUG`

A previously tested build without `-fno-strict-aliasing` hung at startup. Do not omit this option.

以前の検証では `-fno-strict-aliasing` を省略すると起動時に停止しました。必ず指定してください。

```sh
cmake -S Windows_x86_64/Source -B Windows_x86_64/build-local \
  -G "Unix Makefiles" \
  -DCMAKE_SYSTEM_NAME=Windows \
  -DCMAKE_C_COMPILER=x86_64-w64-mingw32-gcc \
  -DCMAKE_CXX_COMPILER=x86_64-w64-mingw32-g++ \
  -DCMAKE_BUILD_TYPE=Release \
  -DCMAKE_C_FLAGS_RELEASE="-O3 -fno-strict-aliasing -DNDEBUG" \
  -DCMAKE_CXX_FLAGS_RELEASE="-O3 -fno-strict-aliasing -DNDEBUG" \
  -DCHDR_WANT_TESTS=OFF
cmake --build Windows_x86_64/build-local --target tsugaru_libretro -j 4
```

Expected / 生成予定: `Windows_x86_64/build-local/libretro/tsugaru_libretro.dll`

## 5. Verification / 生成物の確認

Inspect generated files on the Linux build host. / Linuxビルド環境で生成物を確認してください。

```sh
file Linux_x86_64/build-local/libretro/tsugaru_libretro.so
file Linux_ARM64/build-local/libretro/tsugaru_libretro.so
file Android_ARM64/build-local/libretro/tsugaru_libretro.so
file Windows_x86_64/build-local/libretro/tsugaru_libretro.dll
```

Expected architectures / 想定形式:

| Target / 対象 | Format / 形式 |
|---|---|
| Linux x86_64 | ELF x86-64 |
| Linux ARM64 | ELF AArch64 |
| Android ARM64 | ELF AArch64 for Android |
| Windows x86_64 | PE32+ x86-64 DLL |

A successful build does not guarantee compatibility with every RetroArch installation or device. Test on the intended target.

ビルド成功だけでは、すべてのRetroArch環境や実機での動作を保証できません。対象環境で動作確認してください。

## 6. Notes / 注意事項

- Use each target's own `Source` tree. / 各対象OS専用の `Source` を使用してください。
- Keep build directories separate from the release binaries. / ビルドディレクトリと配布バイナリを分離してください。
- Dependencies may vary by host OS and target device. / 必要な依存ライブラリはビルド環境・実機によって異なります。
- These are documented configurations, not a claim of reproducible byte-identical builds. / 本書はビルド設定の記録であり、バイト単位の再現ビルドを保証するものではありません。
