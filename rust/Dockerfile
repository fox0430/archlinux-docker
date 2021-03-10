FROM archlinux:base-devel

# WORKAROUND for glibc 2.33 and old Docker
RUN patched_glibc=glibc-linux4-2.33-4-x86_64.pkg.tar.zst && \
curl -LO "https://repo.archlinuxcn.org/x86_64/$patched_glibc" && \
bsdtar -C / -xvf "$patched_glibc"

# Get Japan mirrorlist from https://archlinux.org/mirrorlist/
RUN curl "https://archlinux.org/mirrorlist/?country=JP&protocol=http&protocol=https&ip_version=4" -o /etc/pacman.d/mirrorlist && \
    sed -i -e "s/^#//" /etc/pacman.d/mirrorlist

# Add user
ARG USERNAME=arch
ARG UID=1000
ARG GID=1000
RUN groupadd -g ${GID} ${USERNAME} && \
    useradd -u ${UID} -g ${GID} -m -s /bin/bash ${USERNAME} && \
    passwd -d ${USERNAME} && \
    mkdir -p /etc/sudoers.d && \
    echo "${USERNAME} ALL=(ALL) ALL" > /etc/sudoers.d/${USERNAME}

# install yay and some packages
RUN mkdir /build && \
    chown -R ${USERNAME}:${USERNAME} /build
WORKDIR "/build"
RUN pacman -Syyu --noconfirm && \
    pacman -S --noconfirm git go && \
    sudo -u ${USERNAME} git clone --depth 1 https://aur.archlinux.org/yay.git
WORKDIR "/build/yay"
RUN sudo -u ${USERNAME} makepkg --noconfirm -si && \
    rm -rf /home/{$USERNAME}/.cache && \
    pacman -Rs --noconfirm go && \

    pacman -S --noconfirm fish vim rustup && \
    yes | pacman -Scc --noconfirm

RUN rustup default stable

WORKDIR /root

CMD ["/bin/fish"]
