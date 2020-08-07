# vim:set ft=sh et:
# Maintainer: Bartłomiej Piotrowski <nospam@bpiotrowski.pl>
# Contributor: Ionut Biru <ibiru@archlinux.org>
# Contributor: Sébastien Luttringer <seblu@aur.archlinux.org>

pkgbase=virtualbox-modules-bede-lts
pkgname=('virtualbox-modules-bede-lts-host' 'virtualbox-modules-bede-lts-guest')
pkgver=6.1.12
_extramodules=5.4-BEDE-LTS-external
_current_linux_version=5.4.57
_next_linux_version=5.5
pkgrel=7
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
source=('modules-load-virtualbox-bede-lts')
sha512sums=('80bf2c8a402ab066fb70c39a241fa92494c07616fe347bd2ed35b68ddfddab1ceed6322493848f28400fc4706c631e7376646aac0bb572b686bf98aa6d8a8991')


package_virtualbox-modules-bede-lts-host() {
    pkgdesc="Kernel host modules for VirtualBox (linux-bede-lts)"
    license=('GPL')
	depends=(
        "linux-bede-lts>=$_current_linux_version"
        "linux-bede-lts<$_next_linux_version"
    )
    provides=('VIRTUALBOX-HOST-MODULES')

    local kernver="$(</usr/src/linux-bede-lts/version)"
    local extradir="/usr/lib/modules/$kernver/extramodules"

    # when dkms was used
    cd "/var/lib/dkms/vboxhost/${pkgver}_OSE/$kernver/$CARCH/module"
    # when build is used
    #cd dkms/vboxhost/${pkgver}_OSE/$_kernver/$CARCH/module
    install -dm755 "${pkgdir}${extradir}/$pkgname"
    install -Dm644 * "${pkgdir}${extradir}/$pkgname/"
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

    local kernver="$(</usr/src/linux-bede-lts/version)"
    local extradir="/usr/lib/modules/$kernver/extramodules"

    cd "/var/lib/dkms/vboxsf/${pkgver}_OSE/$kernver/$CARCH/module"
    install -dm755 "${pkgdir}${extradir}/$pkgname"
    install -Dm644 * "${pkgdir}${extradir}/$pkgname/"
    find "${pkgdir}" -name '*.ko' -exec xz {} +
}

