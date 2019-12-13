# elkpi-sdk
Yocto cross-compiling toolchains for Elk on Raspberry Pi

This is a meta-repository to host the [Yocto](https://www.yoctoproject.org/) toolchain needed to cross-compile binaries for Elk on Raspberry Pi 3 64bit using your host computer.

The [latest toolchain installer](https://github.com/elk-audio/elkpi-sdk/releases/download/v0.6/elk-glibc-x86_64-elk-sika-image-dev-cortexa7t2hf-neon-vfpv4-raspberrypi3-toolchain-1.0.sh) is available under the [Releases section](https://github.com/elk-audio/elkpi-sdk/releases).

## Instructions

On pretty much any modern Linux distribution, you can simply run the self-extracting shell script and enter the desired extraction path when prompted. Then, simply source the environment script:

```bash
$ source /path/to/environment-setup-cortexa7t2hf-neon-vfpv4-elk-linux-gnueabi
```

and you're ready to go. The environment script modifies the default toolchain for the session, so just e.g. running `make` or `cmake` will generate a binary for the target architecture.

By default, `-g` is added to the C/C++ compiler flags. If you want to compile binaries without symbols, just override the environment variables `CFLAGS` and `CXXFLAGS`, or run `$STRIP` on the generated binary.

## Instructions (macOS / Windows)

If you're not running Linux on your host development machine, the first option is to run a Virtual Machine using e.g. VirtualBox or VMWare. Any recent Linux distribution will be ok to setup the toolchain.

Another option is to use [Docker](https://www.docker.com/) to setup a lightweight container more integrated with your host setup. This could also be useful for Linux if you want to isolate various toolchain environments from each other.

To setup the toolchain with Docker, you can use the [CROPS eSDK container](https://github.com/crops/extsdk-container) following the instructions on the main repository. In this repo you can find [dedicated installation instructions for macOS](running_docker_container_on_macos.md).
