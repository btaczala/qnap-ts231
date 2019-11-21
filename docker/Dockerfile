FROM archlinux/base

RUN echo '[multilib]' >> /etc/pacman.conf && \
    echo 'Include = /etc/pacman.d/mirrorlist' >> /etc/pacman.conf && \
    pacman --noconfirm -Syyu && \
    pacman --noconfirm -S base-devel git && \
    useradd -m -r -s /bin/bash aur && \
    passwd -d aur && \
    echo 'aur ALL=(ALL) ALL' > /etc/sudoers.d/aur && \
    mkdir -p /home/aur/.gnupg && \
    echo 'standard-resolver' > /home/aur/.gnupg/dirmngr.conf && \
    chown -R aur:aur /home/aur && \
    mkdir /build && \
    chown -R aur:aur /build && \
    cd /build && \
    sudo -u aur git clone --depth 1 https://aur.archlinux.org/yay.git && \
    cd yay && \
    sudo -u aur makepkg --noconfirm -si && \
    sudo -u aur yay --afterclean --removemake --save && \
    pacman -Qtdq | xargs -r pacman --noconfirm -Rcns && \
    rm -rf /home/aur/.cache && \
    rm -rf /build


USER aur
RUN gpg --recv-keys 13FCEF89DD9E3C4F
RUN gpg --recv-keys A328C3A2C3C45C06
RUN yay -S cmake wget --noconfirm
RUN cd /home/aur && wget https://releases.linaro.org/components/toolchain/binaries/latest-7/arm-linux-gnueabihf/gcc-linaro-7.2.1-2017.11-i686_arm-linux-gnueabihf.tar.xz
RUN cd /home/aur && tar xf gcc-linaro-7.2.1-2017.11-i686_arm-linux-gnueabihf.tar.xz
USER root
RUN mv /home/aur/gcc-linaro-7.2.1-2017.11-i686_arm-linux-gnueabihf /opt/arm-linux-gnueabihf-7.2.1
USER aur
RUN rm /home/aur/gcc-linaro-7.2.1-2017.11-i686_arm-linux-gnueabihf.tar.xz
RUN yay -S core/lib32-glibc --noconfirm
RUN /opt/arm-linux-gnueabihf-7.2.1/bin/arm-linux-gnueabihf-gcc --version
CMD ["/bin/bash"]
