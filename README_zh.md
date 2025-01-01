# SQLite 构建 for Android

<div align="center">
<strong>
<samp>

[简体中文](README_zh.md) · [English](README.md)

</samp>
</strong>
</div>

此存储库包含为 Android 生成以下版本 SQLite 的脚本:
- 静态链接 命令行 (`sqlite3-static`)
- 动态链接 命令行 (`sqlite3-dynamic`)
- 静态库 (`libsqlite3.a`)
- 动态链接共享对象库 (`libsqlite3.so`)

通过一个命令为以下 ABI 生成二进制文件，文件在以下文件夹中：`armeabi-v7`、`arm64-v8a`、`x86 `、`x86_64`。

## 如何通过 Github Actions 下载编译后的二进制文件

此项目在每次提交时都会触发 GitHub Workflows 自动编译。
可以通过 [Actions tab](https://github.com/zjw2017/sqlite3-android/actions) 方式下载编译后的工件

如果 artifacts 不再可下载，因为距离上次提交已经90多天了，**请随时发送更新 SQLite 版本号的PR**，这样我就可以批准它并运行新工件的生成。

您也可以 fork 该存储库并在您的存储库中手动启动工作流。


## 如何更新 SQLite 版本

从 [官方 SQLite 下载页面](http://www.sqlite.org/download.html) 获取最新的 `sqlite-amalgamation` 下载链接。
从中提取年份和版本，并将它们放入 Makefile。例如:

```bash
vi Makefile

...
SQLITE_VERSION ?= 3460000
SQLITE_YEAR    ?= 2024
...
```

它会为您下载和编译源代码。


## 如何自行编译

- 安装 [Android NDK](https://developer.android.google.cn/ndk/downloads?hl=zh-cn) 并配置您的环境，以便在PATH中找到命令“ndk-build”
对于交叉编译和构建 Android 原生代码应用程序而言，这是一个必要的依赖项。


- 编译命令行和库:
  ```bash
  make clean
  make
  ```

- 预期输出:
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
  [arm64-v8a] Compile        : sqlite3-dynamic-cli <= shell.c
  [arm64-v8a] Compile        : sqlite3-dynamic-cli <= sqlite3.c
  [arm64-v8a] Compile        : sqlite3-so <= sqlite3.c
  [arm64-v8a] SharedLibrary  : libsqlite3.so
  [arm64-v8a] Executable     : sqlite3-dynamic
  [arm64-v8a] Install        : sqlite3-dynamic => libs/arm64-v8a/sqlite3-dynamic
  [arm64-v8a] Install        : libsqlite3.so => libs/arm64-v8a/libsqlite3.so
  [arm64-v8a] Compile        : sqlite3-static-cli <= shell.c
  [arm64-v8a] Compile        : sqlite3-static-cli <= sqlite3.c
  [arm64-v8a] Compile        : sqlite3-a <= sqlite3.c
  [arm64-v8a] StaticLibrary  : libsqlite3.a
  [arm64-v8a] Executable     : sqlite3-static
  [arm64-v8a] Install        : sqlite3-static => libs/arm64-v8a/sqlite3-static
  [x86] Compile        : sqlite3-dynamic-cli <= shell.c
  [x86] Compile        : sqlite3-dynamic-cli <= sqlite3.c
  [x86] Compile        : sqlite3-so <= sqlite3.c
  [x86] SharedLibrary  : libsqlite3.so
  [x86] Executable     : sqlite3-dynamic
  [x86] Install        : sqlite3-dynamic => libs/x86/sqlite3-dynamic
  [x86] Install        : libsqlite3.so => libs/x86/libsqlite3.so
  [x86] Compile        : sqlite3-static-cli <= shell.c
  [x86] Compile        : sqlite3-static-cli <= sqlite3.c
  [x86] Compile        : sqlite3-a <= sqlite3.c
  [x86] StaticLibrary  : libsqlite3.a
  [x86] Executable     : sqlite3-static
  [x86] Install        : sqlite3-static => libs/x86/sqlite3-static
  [x86_64] Compile        : sqlite3-dynamic-cli <= shell.c
  [x86_64] Compile        : sqlite3-dynamic-cli <= sqlite3.c
  [x86_64] Compile        : sqlite3-so <= sqlite3.c
  [x86_64] SharedLibrary  : libsqlite3.so
  [x86_64] Executable     : sqlite3-dynamic
  [x86_64] Install        : sqlite3-dynamic => libs/x86_64/sqlite3-dynamic
  [x86_64] Install        : libsqlite3.so => libs/x86_64/libsqlite3.so
  [x86_64] Compile        : sqlite3-static-cli <= shell.c
  [x86_64] Compile        : sqlite3-static-cli <= sqlite3.c
  [x86_64] Compile        : sqlite3-a <= sqlite3.c
  [x86_64] StaticLibrary  : libsqlite3.a
  [x86_64] Executable     : sqlite3-static
  [x86_64] Install        : sqlite3-static => libs/x86_64/sqlite3-static
  ...
  ```


## Artifacts 位置

- 命令行:
  ```
  libs/armeabi-v7a/sqlite3-static
  libs/armeabi-v7a/sqlite3-dynamic
  libs/arm64-v8a/sqlite3-static
  libs/arm64-v8a/sqlite3-dynamic
  libs/x86_64/sqlite3-static
  libs/x86_64/sqlite3-dynamic
  libs/x86/sqlite3-static
  libs/x86/sqlite3-dynamic
  ```

- 库:
  ```
  obj/local/armeabi-v7a/libsqlite3.a
  obj/local/arm64-v8a/libsqlite3.a
  obj/local/x86_64/libsqlite3.a
  obj/local/x86/libsqlite3.a
  libs/armeabi-v7a/libsqlite3.so
  libs/arm64-v8a/libsqlite3.so
  libs/x86_64/libsqlite3.so
  libs/x86/libsqlite3.so
  ```


## 如何将 SQLite 推送到您的Android设备

将 `arm64-v8a` 替换为您需要的 ABI

```bash
adb push libs/arm64-v8a/sqlite3-static /sdcard/sqlite3
adb shell
mv /sdcard/sqlite3 /data/local/
chmod 755 /data/local/sqlite3
/data/local/sqlite3 -help
```

请注意，路径和权限可能因您的 Android 设备或环境而异。


## Happy hacking!
