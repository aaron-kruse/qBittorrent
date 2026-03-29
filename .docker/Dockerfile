FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive
# Define the required Qt version and installation path
ENV QT_VERSION=6.6.0
ENV QT_INSTALL_DIR=/opt/Qt
# Set the CMAKE_PREFIX_PATH to guide CMake to the installed Qt 6.6.0
ENV CMAKE_PREFIX_PATH=$QT_INSTALL_DIR/$QT_VERSION/gcc_64/lib/cmake
# Qt runtime fix:
ENV QT_QPA_PLATFORM_PLUGIN_PATH=$QT_INSTALL_DIR/$QT_VERSION/gcc_64/plugins/platforms
ENV LD_LIBRARY_PATH=$QT_INSTALL_DIR/$QT_VERSION/gcc_64/lib:$LD_LIBRARY_PATH

# Install all dependencies in one efficient RUN layer
RUN apt-get update && \
    # 1. Install core tools, Python, venv, and ALL required runtime/linking libs
    apt-get install -y \
    build-essential cmake git ninja-build pkg-config \
    python3 python3-pip python3-venv wget xz-utils \
    libglib2.0-0 libpcre2-8-0 \
    libxkbcommon0 libdbus-1-3 \
    libxcb-icccm4 libxcb-image0 libxcb-keysyms1 libxcb-render-util0 libxcb-shm0 libxcb-sync1 libxcb-xfixes0 libxcb-xkb1 libxkbcommon-x11-0 libxcb-cursor0 libxcb-shape0 && \
    \
    # 2. Setup a virtual environment (VENV) and install aqtinstall into it
    python3 -m venv /opt/venv && \
    /opt/venv/bin/pip install aqtinstall && \
    \
    # 3. Install the base Qt 6.6.0 component (gcc_64 architecture)
    /opt/venv/bin/aqt install-qt --outputdir $QT_INSTALL_DIR linux desktop $QT_VERSION gcc_64 && \
    \
    # 4. Install remaining standard native dependencies via apt
    apt-get install -y \
    libboost-dev libssl-dev zlib1g-dev libgl1-mesa-dev \
    libtorrent-rasterbar-dev && \
    \
    # 5. Clean up: Remove the virtual environment and clear apt cache
    rm -rf /opt/venv && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/*

WORKDIR /app