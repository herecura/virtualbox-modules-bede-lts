# vim:set ft=sh et:
# Maintainer: Bartłomiej Piotrowski <nospam@bpiotrowski.pl>
# Contributor: Ionut Biru <ibiru@archlinux.org>
# Contributor: Sébastien Luttringer <seblu@aur.archlinux.org>

pkgbase=virtualbox-modules-bede-lts
pkgname=('virtualbox-modules-bede-lts-host' 'virtualbox-modules-bede-lts-guest')
pkgver=6.0.10
_extramodules=4.19-BEDE-LTS-external
_current_linux_version=4.19.61
_next_linux_version=4.20
pkgrel=3
arch=('x86_64')
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
sha512sums=('e91bca3a219ea2fee594c43a9915d17381675dc3af4f0ba980b64e42fa7df28e38a7fcffa8089d8f859d532ae7b08ac7157afea4f3bf907136cb3abd1b4f4867'
            '2e0a925a2bd13bf4e224ddbf1923effdfe673081e165927e9fc2a75550a2231f5262df26585d9efed79da3adff295cb631dd16831a4ece0ddea6d3b494809707')


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
    find "${pkgdir}" -name '*.ko' -exec xz {} +

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
    cd "/var/lib/dkms/vboxsf/${pkgver}_OSE/$_kernver/$CARCH/module"
    install -m644 * "$pkgdir/usr/lib/modules/$_extramodules/vbox"
    find "${pkgdir}" -name '*.ko' -exec xz {} +

    install -D -m 0644 "$srcdir/60-vboxguest.rules" \
        "$pkgdir/usr/lib/udev/rules.d/60-vboxguest-bede-lts.rules"
}

