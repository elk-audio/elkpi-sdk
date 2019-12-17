# Running Toolchain with Docker on macOS

Docker is a virtualized environment that mimics a Linux setup on your host machine.

The following instructions are adapted from the [CROPS eSDK documenation](https://github.com/crops/extsdk-container/blob/master/README.md) for macOS.

## 1. Install Docker

This should be straightforward using instructions to setup Docker Desktop for macOS on [the official Docker website](https://docs.docker.com/docker-for-mac/).

## 2. Set up Docker Volume and Samba share

Run all the following in a macOS terminal session.

### Create the volume

```bash
docker volume create --name elkvolume
docker run -it --rm -v elkvolume:/workdir busybox chown -R 1000:1000 /workdir
```

### Create and run a samba container

When running a Docker container, your shell environment is virtualized and isolated from the rest of your host filesystem.

You will probably need to share files between the virtual environment and your default filesystem. To do that, you can create a Samba share container and start it with:

```bash
docker create -t -p 445:445 --name samba -v elkvolume:/workdir crops/samba
docker start samba
```

OSX will not let you connect to a locally running samba share. Therefore, create an alias for 127.0.0.1 of 127.0.0.2:

```bash
sudo ifconfig lo0 127.0.0.2 alias up
```

### Copy the toolchain installation script to the Docker Volume

Download the release of the [toolchain installer](https://github.com/elk-audio/elkpi-sdk/releases/) corresponding to the ELK Audio OS version installed in your board.

Then, open _Finder_, hit _⌘+K_ and in the "_Server Address_" field type **`smb://127.0.0.2/workdir`** and click _Connect_ choosing to continue as guest.

You can now copy files to/from the Container Volume by using this folder. Copy the installer toolchain (e.g. `elk-glibc-x86_64-elk-sika-image-dev-cortexa7t2hf-neon-vfpv4-raspberrypi3-toolchain-1.0.sh`) to it to proceed with the installation.

## 3. Install the toolchain

Assuming you have used the paths suggested in the previous steps, run:

```bash
docker run --rm -it -v elkvolume:/workdir crops/extsdk-container --url /workdir/elk-glibc-x86_64-elk-sika-image-dev-cortexa7t2hf-neon-vfpv4-raspberrypi3-toolchain-1.0.sh
```

(Note that you might have to replace the filename of the toolchain istaller with the one downloaded and copied to the volume)

This will exctract and setup the toolchain in your Docker container. The whole process should take few minutes and when finished you will have access to a shell with the environment properly set up for cross-compilation.

## 4. Run the toolchain

After installation, you can quickly setup the toolchain again from a new terminal by typing:

```bash
docker start samba && sudo ifconfig lo0 127.0.0.2 alias up
docker run --rm -it -v elkvolume:/workdir crops/extsdk-container
```

