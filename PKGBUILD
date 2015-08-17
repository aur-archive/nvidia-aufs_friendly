# Contributor : Thomas Baechler <thomas@archlinux.org>
# Contributor : Daniel YC Lin <dlin.tw at gmail>
# Maintainer : Jesse Sherlock <jesse at jessesherlock.com>

pkgname=nvidia-aufs_friendly
pkgver=331.20
pkgrel=2
_goodkver=3.12
_badkver=3.13
_extramodules=extramodules-${_goodkver}-aufs_friendly
pkgdesc="NVIDIA drivers for linux-aufs_friendly"
arch=('i686' 'x86_64')
url="http://www.nvidia.com/"
depends=("linux-aufs_friendly>=${_goodkver}" "linux-aufs_friendly<${_badkver}" "nvidia-libgl" "nvidia-utils=${pkgver}")
makedepends=("linux-aufs_friendly-headers>=${_goodkver}" "linux-aufs_friendly-headers<${_badkver}")
conflicts=('nvidia-96xx' 'nvidia-173xx' 'nvidia-304xx-aufs_friendly')
license=('custom')
install=nvidia.install
options=(!strip)
md5sums=('')

if [ "$CARCH" = "i686" ]; then
    _arch='x86'
    _pkg="NVIDIA-Linux-${_arch}-${pkgver}"
    source+=("ftp://download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run")
    md5sums+=('801aa04a087891690f1cac09575b2ba9')
elif [ "$CARCH" = "x86_64" ]; then
    _arch='x86_64'
    _pkg="NVIDIA-Linux-${_arch}-${pkgver}-no-compat32"
    source+=("ftp://download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run")
    md5sums+=('28295eed56c2ca996401c0093279620f')
fi

build() {
    _kernver="$(cat /usr/lib/modules/${_extramodules}/version)"
    cd "${srcdir}"
    sh "${_pkg}.run" --extract-only
    cd "${_pkg}/kernel"
    make SYSSRC=/usr/lib/modules/"${_kernver}/build" module
}

package() {
    install -D -m644 "${srcdir}/${_pkg}/kernel/nvidia.ko" \
        "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia.ko"
    install -d -m755 "${pkgdir}/usr/lib/modprobe.d"
    echo "blacklist nouveau" >> "${pkgdir}/usr/lib/modprobe.d/nvidia-aufs_friendly.conf"
    sed -i -e "s/EXTRAMODULES='.*'/EXTRAMODULES='${_extramodules}'/" "${startdir}/nvidia.install"
    gzip "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia.ko"
}
