# BlackDroid
BlackArch XFCE on Termux

## Installer script:

```sh
# This is a proot-distro plugin for installing BlackArch with XFCE in Termux.

DISTRO_NAME="BlackArch Linux"
DISTRO_COMMENT="BlackArch Linux is an Arch Linux-based distribution for penetration testers and security researchers."

TARBALL_URL['aarch64']="https://github.com/termux/proot-distro/releases/download/v4.17.3/archlinux-aarch64-pd-v4.17.3.tar.xz"
TARBALL_SHA256['aarch64']="dc56b998ffa2663209417396c8d70caf87c8052acf41e9a2c6daf24cbd181533"
TARBALL_URL['arm']="https://github.com/termux/proot-distro/releases/download/v4.17.3/archlinux-arm-pd-v4.17.3.tar.xz"
TARBALL_SHA256['arm']="4b698018ded0656e17c0867b97b53cc32be5906c0f37e02ab499c65d5f12d439"
TARBALL_URL['i686']="https://github.com/termux/proot-distro/releases/download/v4.17.3/archlinux-i686-pd-v4.17.3.tar.xz"
TARBALL_SHA256['i686']="787d43c21fae2c6efe843a324ff2875fc654fe8475020deb8678c224967f29af"
TARBALL_URL['x86_64']="https://github.com/termux/proot-distro/releases/download/v4.17.3/archlinux-x86_64-pd-v4.17.3.tar.xz"
TARBALL_SHA256['x86_64']="75e069c3f59f4806848972dfd6a2d390b0328ca3f4486db140eb21d1d376b35b"

distro_setup() {
    # Fix environment variables on login or su.
    local f
    for f in su su -l system-local-login system-remote-login; do
        echo "session  required  pam_env.so readenv=1" >> ./etc/pam.d/"${f}"
    done

    # Configure en_US.UTF-8 locale.
    sed -i -E 's/#[[:space:]]?(en_US.UTF-8[[:space:]]+UTF-8)/\1/g' ./etc/locale.gen
    run_proot_cmd locale-gen

    # Install BlackArch
    run_proot_cmd "curl -O https://blackarch.org/strap.sh"
    run_proot_cmd "chmod +x strap.sh"
    run_proot_cmd "./strap.sh"
    run_proot_cmd "pacman -Syu --noconfirm"
    run_proot_cmd "pacman -S --noconfirm blackarch"
    run_proot_cmd "pacman -S --noconfirm xfce4 xfce4-goodies"
}

# Run the setup
distro_setup
```

You can save this script as `blackarch.sh` in the `distro-plugins` directory of the `proot-distro` repository. This script will install BlackArch with XFCE in Termux using proot-distro.

