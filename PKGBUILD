# Maintainer: Chris Speck  <chris.speck(at)annalise(dot)ai>

pkgname=slack-desktop-wayland
_original_pkgname=slack-desktop
pkgver=4.39.88
pkgrel=1
pkgdesc="Slack Desktop (Beta) for Linux with Wayland Support"
arch=('x86_64')
url="https://slack.com/downloads"
license=('custom')
depends=(
    'electron'
    'gtk3'
    'libsecret'
    'libxss'
    'nss'
    'pipewire'
    'xdg-desktop-portal'
    'xdg-utils'
)
optdepends=(
    'libappindicator-gtk3: Systray indicator support'
    'org.freedesktop.secrets: Keyring password store support'
    'xdg-desktop-portal-gnome: xdg-desktop-portal support for GNOME'
    'xdg-desktop-portal-gtk: xdg-desktop-portal support for GNOME and GTK'
    'xdg-desktop-portal-kde: xdg-desktop-portal support for KDE'
    'xdg-desktop-portal-lxqt: xdg-desktop-portal support for LXQt'
    'xdg-desktop-portal-wlr: xdg-desktop-portal support for wlroots-based Wayland compositors'
)

source=(
    "$pkgname-$pkgver.deb::https://downloads.slack-edge.com/desktop-releases/linux/x64/$pkgver/slack-desktop-$pkgver-amd64.deb"
    'slack.sh'
)

noextract=("${_original_pkgname}-${pkgver}-amd64.deb")
b2sums=('09d0383444b342c8d0d14cd7c6a3cfb4f32ff1eeabed883b7c62cf42ed7859d7796abe99a0ad9d1602aac183182d0a02931282318e358e6e1b1b1956d03a29f5'
    '8bd96010498a259259c0580b4a1900079af839fad0f036902aaac39731ecc79fdc7d35d059d5b9c1b330edd02d34414b9140262e60b52faacb19747ff2962f82')
provides=('slack-desktop')
conflicts=('slack-desktop' 'slack-electron')

prepare() {
    bsdtar -xf data.tar.xz

    # Enable slack silent mode and fix icon
    sed -ri \
        -e 's|^(Exec=.+/slack)(.+)|\1 -s\2|' \
        -e 's/^Icon=.+slack\.png/Icon=slack/' \
        "usr/share/applications/slack.desktop"

    # patch the asar file to fix/enable pipewire
    # see https://github.com/flathub/com.slack.Slack/issues/101#issuecomment-1807073763
    sed -i -e 's/,"WebRTCPipeWireCapturer"/,"_ebRTCPipeWireCapturer"/' usr/lib/slack/resources/app.asar
}

package() {
    install -Dv "slack.sh" "$pkgdir/usr/bin/slack"
    install -dv "$pkgdir/usr/lib/slack/"
    cp -av --no-preserve=ownership usr/lib/slack/resources/* "$pkgdir/usr/lib/slack/"
    install -Dvm644 "usr/share/applications/slack.desktop" -t "$pkgdir/usr/share/applications"
    install -Dvm644 "usr/share/pixmaps/slack.png" -t "$pkgdir/usr/share/pixmaps"
    install -Dvm644 "usr/lib/slack/LICENSE" -t "$pkgdir/usr/share/licenses/$pkgname/"
}
