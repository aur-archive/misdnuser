# Maintainer:
# Contributor: Tobias Powalowski <tpowa@archlinux.org>

pkgname=misdnuser
pkgver=2.0.17_20120917
pkgrel=2
pkgdesc="Tools and library for mISDN"
arch=('i686' 'x86_64')
url="http://www.misdn.org"
license=('GPL')
depends=('isdn4k-utils' 'spandsp')
backup=('etc/capi20.conf') 
options=('!makeflags' '!libtool' '!strip')
source=(ftp://ftp.archlinux.org/other/misdnuser/${pkgname}-${pkgver}.tar.gz
        mISDNcapid.service
        c2faxrecv-mISDNcapid.service
        mISDNcapid.conf)
md5sums=('4cdb52f2c0ff1a1426573ac5ef09e9f8'
         '8f305ee6e35fa12a8bec0632bfe7a076'
         '32ead7f063e007c784aa883d441f33c2'
         '42c3b46880a68c3883ee1ed00af34b45')

build() {
  # only enable for debugging!
  #export CFLAGS+=" -g -O0"
  #export CXXFLAGS+=" -g -O0"
  cd ${srcdir}/${pkgname}-${pkgver}
  make
  ./configure --prefix=/usr --sysconfdir=/etc --enable-capi --enable-softdsp --with-mISDN_group=uucp
  make
}

package() {
  cd ${srcdir}/${pkgname}-${pkgver}
  make DESTDIR=${pkgdir} install
  # fix udev rule
  mkdir -p ${pkgdir}/usr/lib/udev/rules.d
  mv ${pkgdir}/etc/udev/rules.d/45-misdn.rules ${pkgdir}/usr/lib/udev/rules.d
  rm -r ${pkgdir}/etc/udev/
  # add systemd files
  install -D -m644 ${srcdir}/mISDNcapid.service ${pkgdir}/usr/lib/systemd/system/mISDNcapid.service
  # mISDNcapid:
  # tends to crash on avmfritz card, add an extra systemd file for
  # restarting the services until segfaults are fixed!
  install -D -m644 ${srcdir}/c2faxrecv-mISDNcapid.service ${pkgdir}/usr/lib/systemd/system/c2faxrecv-mISDNcapid.service
  install -D -m644 ${srcdir}/mISDNcapid.conf ${pkgdir}/usr/lib/tmpfiles.d/mISDNcapid.conf
}
