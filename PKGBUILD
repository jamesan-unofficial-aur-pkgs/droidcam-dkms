# Maintainer: James An <james@jamesan.ca>
# Contributor: AwesomeHaircut <jesusbalbastro at gmail com>

pkgname=droidcam-dkms
_pkgname=${pkgname%-dkms}
pkgver=6.0
pkgrel=3
pkgdesc='A tool for using your android device as a wireless/usb webcam'
arch=('x86_64')
url='http://www.dev47apps.com'
license=('custom')
depends=('bluez-libs' 'dkms' 'gtk2')
optdepends=('v4l-utils: Userspace tools and conversion library for Video 4 Linux'
            'xf86-video-v4l: X.org v4l video driver' )
provides=("$_pkgname=$pkgver")
conflicts=("$_pkgname")
options=()
install="$_pkgname.install"
source=("$_pkgname.desktop"
        dkms.conf
        Makefile.patch
        "http://files.dev47apps.net/600/$_pkgname-v4l2-x64.tar.gz")
md5sums=('199d8f3dbc6697f06350b00de99f2274'
         'e01bbf653b83b6b231391c3d99a54d68'
         '6f2e74a8921ffa0eaef2759fdf44f595'
         '8a81b4f9b61ce98a34d8a916e7be9d5e')

build() {
 patch -p1 -i Makefile.patch
}

package() {
  # Install droidcam binary file
  mkdir -p "$pkgdir"/usr/bin
  install -m755 $_pkgname "$pkgdir"/usr/bin/$_pkgname
  install -m755 $_pkgname-cli "$pkgdir"/usr/bin/$_pkgname-cli

  # Install the desktop icon and ".desktop" files
  install -dm0755 "${pkgdir}/usr/share/"{applications,pixmaps}
  install -m0644 icon2.png "${pkgdir}/usr/share/pixmaps/$_pkgname.png"
  install -m0644 "$_pkgname.desktop" "${pkgdir}/usr/share/applications/$_pkgname.desktop"

  # Install
  msg2 "Starting make install..."
  install -Dm644 v4l2loopback/v4l2loopback-dc.c "${pkgdir}"/usr/src/v4l2loopback-0.6.1/v4l2loopback-dc.c

  # Copy dkms.conf
  install -Dm644 dkms.conf "${pkgdir}"/usr/src/v4l2loopback-0.6.1/dkms.conf

  # Set name and version
  #~ sed -e "s/@_PKGBASE@/v4l2loopback/" \
      #~ -e "s/@PKGVER@/0.6.1/" \
      #~ -i "${pkgdir}"/usr/src/v4l2loopback-0.6.1/dkms.conf

  # Copy sources (including Makefile)
  install -Dm644 v4l2loopback/Makefile "${pkgdir}"/usr/src/v4l2loopback-0.6.1/Makefile
  install -Dm644 v4l2loopback/v4l2loopback-dc.c "${pkgdir}"/usr/src/v4l2loopback-0.6.1/v4l2loopback-dc.c

  mkdir -p "$pkgdir"/usr/lib/modules-load.d/
  printf "videodev\nv4l2loopback\nv4l2loopback_dc" \
         > "$pkgdir"/usr/lib/modules-load.d/droidcam.conf

  mkdir -p "$pkgdir"/etc/modprobe.d/
  printf "options v4l2loopback_dc width=320 height=240" \
         > "$pkgdir"/etc/modprobe.d/droidcam.conf

  # Install doc
  install -dm0755 "${pkgdir}/usr/share/licenses/$pkgname"
  install -m0644 "${srcdir}/README" "${pkgdir}/usr/share/licenses/$pkgname/README"
}

