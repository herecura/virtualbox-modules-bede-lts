# vim:set ft=sh et:
# $Id$
# Maintainer: Bartłomiej Piotrowski <nospam@bpiotrowski.pl>
# Contributor: Ionut Biru <ibiru@archlinux.org>
# Contributor: Sébastien Luttringer <seblu@aur.archlinux.org>

pkgbase=virtualbox-modules-bede-lts
pkgname=('virtualbox-modules-bede-lts-host' 'virtualbox-modules-bede-lts-guest')
pkgver=5.1.22
_extramodules=4.9-BEDE-LTS-external
_current_linux_version=4.9.30
_next_linux_version=4.10
pkgrel=7
arch=('i686' 'x86_64')
url='http://virtualbox.org'
license=('GPL')
makedepends=(
    "linux-bede-lts>=$_current_linux_version"
    "linux-bede-lts-headers>=$_current_linux_version"
    "linux-bede-lts<$_next_linux_version"
    "linux-bede-lts-headers<$_next_linux_version"
    "virtualbox-host-dkms>=$pkgver"
    "virtualbox-guest-dkms>=$pkgver"
)
source=('modules-load-virtualbox-bede-lts'
    '60-vboxguest.rules')
sha256sums=('9b3c4dc5385fb3b4aeb841043384879c5c7ee926f5343d6a4177e913604f869d'
            '033c597e0f5285d2ddb0490868e5b6f945f45c7b1b1152a02a9e6fea438b2c95')


package_virtualbox-modules-bede-lts-host() {
    pkgdesc="Kernel host modules for VirtualBox (linux-bede-lts)"
    license=('GPL')
	depends=(
        "linux-bede-lts>=$_current_linux_version"
        "linux-bede-lts<$_next_linux_version"
    )
    provides=('VIRTUALBOX-HOST-MODULES')

    _kernver="$(cat /usr/lib/modules/${_extramodules}/version)"

    install -dm755 "$pkgdir/usr/lib/modules/$_extramodules/vbox"
    cd "/var/lib/dkms/vboxhost/${pkgver}_OSE/$_kernver/$CARCH/module"
    install -m644 * "$pkgdir/usr/lib/modules/$_extramodules/vbox"
    find "$pkgdir" -name '*.ko' -exec gzip -9 {} +

    # install config file in modules-load.d for out of the box experience
    install -Dm644 "$srcdir/modules-load-virtualbox-bede-lts" \
        "$pkgdir/usr/lib/modules-load.d/virtualbox-modules-bede-lts-host.conf"
}

package_virtualbox-modules-bede-lts-guest() {
    pkgdesc="Kernel guest modules for VirtualBox (linux-bede-lts)"
    license=('GPL')
	depends=(
        "linux-bede-lts>=$_current_linux_version"
        "linux-bede-lts<$_next_linux_version"
    )
    provides=('VIRTUALBOX-GUEST-MODULES')

    _kernver="$(cat /usr/lib/modules/${_extramodules}/version)"

    install -dm755 "$pkgdir/usr/lib/modules/$_extramodules/vbox"
    cd "/var/lib/dkms/vboxguest/${pkgver}_OSE/$_kernver/$CARCH/module"
    install -m644 * "$pkgdir/usr/lib/modules/$_extramodules/vbox"
    find "$pkgdir" -name '*.ko' -exec gzip -9 {} +

    install -D -m 0644 "$srcdir/60-vboxguest.rules" \
        "$pkgdir/usr/lib/udev/rules.d/60-vboxguest-bede-lts.rules"
}

