pkgname=ipsec-tools-wild-char
relname=ipsec-tools
pkgver=0.8.1
pkgrel=1
pkgdesc="KAME IPSec tools ported to Linux"
arch=('i686' 'x86_64')
url="http://ipsec-tools.sourceforge.net/"
depends=('readline' 'openssl' 'krb5')
makedepends=('linux-headers')
license=('GPL')
options=('!makeflags' '!libtool')
source=(http://downloads.sourceforge.net/sourceforge/ipsec-tools/$relname-$pkgver.tar.bz2
	racoon.rc
	ipsec.rc
	racoon.service
	ipsec.service
	ipsec-tools-linux-3.7-compat.patch
	wild-char.patch)
md5sums=('d38b39f291ba2962387c3232e7335dd8'
         '416b8e362d86987b8c55f7153cdafbeb'
         '90d0810267cbd847383ae3101699b192'
         '1632fce55ba5592dea1f8bf661106e7d'
         '5bf7478590c751b465617681a31619fe'
         'ae1dd20c83dcfce3dedb46ee73e83613'
	 '3360bedf07793382a04ece37b55b4f68')

build() {
  cd $srcdir/$relname-$pkgver

  patch -p1 <$srcdir/ipsec-tools-linux-3.7-compat.patch
  patch -p1 <$srcdir/wild-char.patch
  sed -i 's#-Werror##' configure.ac

  ./bootstrap
  ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var \
	--enable-security-context=no --enable-hybrid --enable-dpd --enable-natt \
	--enable-adminport --enable-gssapi
  make
}

package() {
  cd $srcdir/$relname-$pkgver
  make DESTDIR=$pkgdir install
  install -Dm0755 $srcdir/racoon.rc $pkgdir/etc/rc.d/racoon
  install -Dm0755 $srcdir/ipsec.rc $pkgdir/etc/rc.d/ipsec
  install -Dm0644 $srcdir/racoon.service $pkgdir/usr/lib/systemd/system/racoon.service
  install -Dm0644 $srcdir/ipsec.service $pkgdir/usr/lib/systemd/system/ipsec.service
}
