FROM archlinux:base-devel

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
