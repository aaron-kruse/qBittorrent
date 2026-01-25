qBittorrent - A BitTorrent client in Qt
------------------------------------------

[![GitHub Actions CI Status](https://github.com/qbittorrent/qBittorrent/actions/workflows/ci_ubuntu.yaml/badge.svg)](https://github.com/qbittorrent/qBittorrent/actions)
[![Coverity Status](https://scan.coverity.com/projects/5494/badge.svg)](https://scan.coverity.com/projects/5494)
********************************
### Personal dev environment notes:
Development was done on a Windows machine using a Docker container
to isolate the dev dependencies to make cleanup easier. Code is
managed on the Windows host then builds/testing happen from the
container. This setup requires [Docker Desktop](https://www.docker.com/products/docker-desktop/)
for the container and [VcXsrv Windows X Server](https://sourceforge.net/projects/vcxsrv/)
to test the Linux-based GUI app from the Windows host.

TODO: simply these steps at some point by using `docker-compose.yaml`.

#### Initial container setup (one-time)
To set the container up for the first time, run:
```
# Create the initial container ("qbittorrent-builder")
docker build -t qbittorrent-builder .
```

#### Building/testing changes (ongoing)
Run the container, configure/compile the code, and run the executable.
The first build will take a while (ex. maybe an hour) but subsequent
builds will be much quicker. The X server must be started on the host
(display 0) before running the executable. The steps below will leave
you in the container when finished (where you can simply rebuild/rerun
to test additional changes or exit out of the container if you're finished).
```
# Start/run the container (note: run with PowerShell)
$HostIP = "192.168.1.143"
docker run -it --rm --name qbittorrent_dev_env `
  -e DISPLAY="${HostIP}:0" `
  -v /tmp/.X11-unix:/tmp/.X11-unix `
  -v ${PWD}:/app/source `
  qbittorrent-builder bash

# Use a build directory under the mounted volume so it's persisted on the host (outside the container)
mkdir -p /app/source/build
cd /app/source/build

# Configure
cmake /app/source -G "Ninja" -DCMAKE_BUILD_TYPE=Release -DBUILD_GUI=ON

# Compile
ninja

# Run (compiled file is at /app/source/build/qbittorrent)
./qbittorrent
```

#### Final cleanup
Stop/remove the container when you no longer need it:
```
docker stop qbittorrent_dev_env
docker rm qbittorrent_dev_env
docker rmi qbittorrent-builder
```

### Description:
qBittorrent is a bittorrent client programmed in C++ / Qt that uses
libtorrent (sometimes called libtorrent-rasterbar) by Arvid Norberg.

It aims to be a good alternative to all other bittorrent clients
out there. qBittorrent is fast, stable and provides unicode
support as well as many features.

The free [IP to Country Lite database](https://db-ip.com/db/download/ip-to-country-lite) by [DB-IP](https://db-ip.com/) is used for resolving the countries of peers. The database is licensed under the [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).

### Installation:

Refer to the [INSTALL](INSTALL) file.

### Public key:
Starting from v3.3.4 all source tarballs and binaries are signed.<br />
The key currently used is 4096R/[5B7CC9A2](https://pgp.mit.edu/pks/lookup?op=get&search=0x6E4A2D025B7CC9A2) with fingerprint `D8F3DA77AAC6741053599C136E4A2D025B7CC9A2`.<br />
You can also download it from [here](https://github.com/qbittorrent/qBittorrent/raw/master/5B7CC9A2.asc).<br />
**PREVIOUSLY** the following key was used to sign the v3.3.4 source tarballs and v3.3.4 Windows installer **only**: 4096R/[520EC6F6](https://pgp.mit.edu/pks/lookup?op=get&search=0xA1ACCAE4520EC6F6) with fingerprint `F4A5FD201B117B1C2AB590E2A1ACCAE4520EC6F6`.<br />

### Misc:
For more information please visit:
https://www.qbittorrent.org

or our wiki here:
https://wiki.qbittorrent.org

Use the forum for troubleshooting before reporting bugs:
https://forum.qbittorrent.org

Please report any bug (or feature request) to:
https://bugs.qbittorrent.org

Official IRC channel:
[#qbittorrent on irc.libera.chat](ircs://irc.libera.chat:6697/qbittorrent)

------------------------------------------
sledgehammer999 \<sledgehammer999@qbittorrent.org\>
