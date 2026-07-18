########################################################################
# StreamFab-Docker
# Runs StreamFab (Windows binary, via Wine) in a browser-accessible
# desktop, based on linuxserver.io's Webtop image.
#
# IMPORTANT:
#  - StreamFab is Windows/Mac/Android only — there is no native Linux
#    build. Wine is the community-recommended way to run it on Linux.
#  - This image does NOT contain StreamFab. You provide your own
#    installer (which you're licensed to run) via the installer/ folder,
#    and log in / activate your license inside the app as normal.
#  - StreamFab has its own built-in Chromium-based browser for capturing
#    streams. That's the part most likely to have Wine quirks (GPU
#    acceleration / sandboxing) — see README for what to check.
########################################################################

FROM lscr.io/linuxserver/webtop:ubuntu-xfce

LABEL maintainer="vale"
LABEL description="StreamFab (via Wine) in a web-accessible desktop, for Unraid"

RUN dpkg --add-architecture i386 && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        software-properties-common \
        wget gnupg2 ca-certificates cabextract unzip xdotool p7zip-full \
    && mkdir -pm755 /etc/apt/keyrings \
    && wget -qO- https://dl.winehq.org/wine-builds/winehq.key | gpg --dearmor -o /etc/apt/keyrings/winehq-archive.gpg \
    && wget -O /tmp/winehq-noble.sources https://dl.winehq.org/wine-builds/ubuntu/dists/noble/winehq-noble.sources \
    && sed 's#/etc/apt/keyrings/winehq-archive.key#/etc/apt/keyrings/winehq-archive.gpg#' /tmp/winehq-noble.sources > /etc/apt/sources.list.d/winehq-noble.sources \
    && rm -f /tmp/winehq-noble.sources \
    && apt-get update \
    && apt-get install -y --install-recommends winehq-stable winetricks \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

ENV WINEPREFIX=/config/.wine
ENV WINEARCH=win64
ENV WINEDEBUG=-all
# Where you drop the StreamFab installer .exe before first boot
ENV STREAMFAB_INSTALLER_DIR=/config/installer
# Where downloaded/recorded output should land (map this to your Unraid share)
ENV STREAMFAB_OUTPUT_DIR=/output

COPY root/ /

VOLUME ["/config", "/output"]

# Webtop already exposes 3000 (HTTP) / 3001 (HTTPS) via its own s6 services
