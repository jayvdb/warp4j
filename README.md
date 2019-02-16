# Warp4j

Turn JAR (java archive) into self-contained executable in a single command.

## Features

- downloads java runtimes automatically
- makes runtimes optimized for your application
- creates self-contained binaries for Linux, macOS and Windows using [warp-packer](https://github.com/dgiagio/warp)
- works on Linux, macOS and Windows (with Windows Subsystem for Linux)
- supports cross "compilation"
- does not require neither JDK nor JRE installed

## TL;DR

Just put both `warp4j` and `warp-packer` somewhere in your PATH and run `warp4j app.jar`.

## Install

### One-liner

Installs latest versions of both `warp4j` and `warp-packer`:

```
$ bash -c "$(curl -fsSL https://raw.githubusercontent.com/guziks/warp4j/master/install)"
```

### Manual

First install [warp-packer](https://github.com/dgiagio/warp/releases) and ensure other common tools available: `awk`, `curl`, `file`, `grep`, `sed`, `sort`, `tar`, `unzip`, optional: `zip`. Then install `warp4j` like this:

```
$ LOCATION=/usr/local/bin \
LINK=https://raw.githubusercontent.com/guziks/warp4j/master/warp4j \
TEMP_LOCATION=/tmp/warp4j \
bash -c 'curl -fsSL -o $TEMP_LOCATION $LINK && \
sudo install -D \
  --mode=755 \
  --owner=root \
  --group=root \
  "$TEMP_LOCATION" "$LOCATION"'
```

Previous command can be also used to upgrade to the latest version.

## Usage

```
$ ls
my-app.jar

$ warp4j my-app.jar
...
...
...

$ ls -1
my-app.jar
warped

$ ls warped
my-app-linux
my-app-linux-x64.tar.gz
my-app-macos
my-app-macos-x64.tar.gz
my-app-windows.exe
my-app-windows-x64.zip
```

See help:

```
$ wapr4j --help

Usage: warp4j [options] <app.jar>

Turn JAR (java archive) into self-contained executable

Options:
  -j, --java-version   <version>
                       override JDK/JRE version
                       examples: "11", "11.0", "11.0.2", "11.0.2+9"
                       (default: 11)
  -o, --output         <directory>
                       override output directory;
                       this is relative to current PWD
                       (default: ./warped)
  --no-optimize        use JRE instead of optimized JDK;
                       by default jdeps and jlink are used to create
                       optimized JDK for the partiular jar,
                       JRE is always used for java 8
  --pull               check if more recent JDK/JRE distro is available;
                       by default latest cached version that matches
                       "--java-version" is used
  --target-linux       create binary for Linux
  --target-macos       create binary for macOS
  --target-windows     create binary for Windows
                       if no targets are specified then binaries for
                       all targets are created
  --jvm-impl           jvm implementation: hotspot or openj9
                       (default: hotspot)
  --jvm-options        <options>
                       passed to java like this:
                       "java <options> -jar <jar file>"
                       use quotes when passing multiple options
                       example: '-Xms512m -Xmx1024m'
  -h, --help           show this message
```

## Compatibility

Tested on the following operating systems:

- Ubuntu 18.04
- macOS Mojave
- Windows Subsystem for Linux with Ubuntu 14.04

## Cache Location

Downloaded runtimes and prepared bundles are here:

- Linux: `$HOME/.local/share/warp4j`
- macOS: `$HOME/Library/Application Support/warp4j`
