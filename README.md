# SQLite build scripts for Android

<div align="center">
<strong>
<samp>

[简体中文](README_zh.md) · [English](README.md)

</samp>
</strong>
</div>

This repository contains scripts to generate the following SQLite artifacts for Android:
- Statically Linked CLI (`sqlite3-static`)
- Dynamically Linked CLI (`sqlite3-dynamic`)
- Static Library (`libsqlite3.a`)
- Dynamically Linked Shared Object Library (`libsqlite3.so`)

The compilation generates, with a single command, binaries for the following ABIs (each in a separate folder): `armeabi-v7a`, `arm64-v8a`, `x86`, `x86_64`.


## How to just download compiled binaries via Github Actions

This project is automatically compiled at each commit via the GitHub workflows contained in the `.github` folder.
It is possible to download compiled artifacts via the [Actions tab](https://github.com/jacopotediosi/sqlite3-android/actions).

If the artifacts are no longer downloadable because it's been more than 90 days since the last commit, *feel free to send a PR updating the sqlite version number*, so that I can approve it and run the generation of the new artifacts.

You can also fork and manually start workflows in your repository.


## How to update the sqlite version

Get the latest `sqlite-amalgamation` download link from [the official sqlite download page](http://www.sqlite.org/download.html).
Extract year and version from it and put them into the Makefile as follows:

```bash
vi Makefile

...
SQLITE_VERSION ?= 3410200
SQLITE_YEAR    ?= 2023
...
```

It will take care to download and compile sources for you.


## How to compile on your own

- Install the [Android NDK](https://developer.android.com/ndk/downloads/index.html) and configure your environment so the command `ndk-build` can be found in the PATH.
This is a build dependency and is necessary to cross-compile and build Android Native Code Applications.

- Make CLI and Library:
  ```bash
  make clean
  make
  ```

- Expected output:
  ```bash
  ...
  [armeabi-v7a] Compile thumb  : sqlite3-dynamic-cli <= shell.c
  [armeabi-v7a] Compile thumb  : sqlite3-dynamic-cli <= sqlite3.c
  [armeabi-v7a] Compile thumb  : sqlite3-so <= sqlite3.c
  [armeabi-v7a] SharedLibrary  : libsqlite3.so
  [armeabi-v7a] Executable     : sqlite3-dynamic
  [armeabi-v7a] Install        : sqlite3-dynamic => libs/armeabi-v7a/sqlite3-dynamic
  [armeabi-v7a] Install        : libsqlite3.so => libs/armeabi-v7a/libsqlite3.so
  [armeabi-v7a] Compile thumb  : sqlite3-static-cli <= shell.c
  [armeabi-v7a] Compile thumb  : sqlite3-static-cli <= sqlite3.c
  [armeabi-v7a] Compile thumb  : sqlite3-a <= sqlite3.c
  [armeabi-v7a] StaticLibrary  : libsqlite3.a
  [armeabi-v7a] Executable     : sqlite3-static
  [armeabi-v7a] Install        : sqlite3-static => libs/armeabi-v7a/sqlite3-static
  ...
  ```


## Artifacts location

- CLI:
  ```
  libs/armeabi-v7a/sqlite3-static
  libs/armeabi-v7a/sqlite3-dynamic
  ... (other ABIs also) ...
  ```

- Library:
  ```
  obj/local/armeabi-v7a/libsqlite3.a
  libs/armeabi-v7a/libsqlite3.so
  ... (other ABIs also) ...
  ```


## How to push sqlite to your Android device

Replacing `armeabi-v7a` with your needed ABI:

```bash
adb push libs/armeabi-v7a/sqlite3-static /sdcard/sqlite3
adb shell
mv /sdcard/sqlite3 /data/local/
chmod 755 /data/local/sqlite3
/data/local/sqlite3 -help
```

Note that paths and permissions may vary in your Android device or environment.


## Happy hacking!
