FROM base/archlinux
MAINTAINER Bruno Adelé "bruno@adele.im"

USER root
ENV DISPLAY :0

# Add yaourt packages repository
ADD pacman.conf /etc/pacman.conf

# Upgrade the packages
RUN pacman-key --init
RUN pacman-key --populate archlinux
RUN pacman-key --refresh-keys
RUN pacman --noconfirm -Syyu
RUN pacman-db-upgrade

# Add user and install sudo
RUN pacman --noconfirm -S sudo
ADD sudoers /etc/sudoers
RUN useradd arch
RUN echo "arch:arch" | chpasswd

# Install yaourt
RUN pacman --noconfirm -S yaourt rsync

# Clean the packages cache
RUN rm -f /var/cache/pacman/pkg/*

USER arch
CMD bash