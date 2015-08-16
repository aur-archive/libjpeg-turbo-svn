# Maintainer: Simone Sclavi 'Ito' <darkhado@gmail.com>
# Contributor: Ross Melin <rdmelin@yahoo.com>
## based on the libjpeg-turbo PKGBUILD

pkgname=libjpeg-turbo-svn
pkgver=856
pkgrel=1
pkgdesc="A derivative of libjpeg which uses SIMD instructions to accelerate baseline JPEG compression and decompression"
arch=('i686' 'x86_64')
url="http://libjpeg-turbo.virtualgl.org/"
license=('GPL' 'custom')
makedepends=('subversion' 'nasm')
provides=('libjpeg-turbo' 'libjpeg=8')
conflicts=('libjpeg-turbo')
options=('!libtool')
depends=('glibc')
md5sums=('8b3f4d426b97ea335b053353f9f220b2') 
_svntrunk=https://libjpeg-turbo.svn.sourceforge.net/svnroot/libjpeg-turbo/trunk
_svnmod=libjpeg-turbo

build() {
  if [ -d $_svnmod/.svn ]; then
    (cd $_svnmod && svn up -r $pkgver)
  else
    svn co $_svntrunk --config-dir ./ -r $pkgver $_svnmod
  fi

  msg "SVN checkout done or server timeout"
  msg "Starting make..."

  rm -rf $_svnmod-build
  cp -r $_svnmod $_svnmod-build
  cd $_svnmod-build

  # put documentation in a reasonable directory...
  sed -i "/docdir = /s#/doc#/doc/libjpeg-turbo-svn#" Makefile.am
  sed -i "/exampledir = /s#/doc#/doc/libjpeg-turbo-svn#" Makefile.am

  # BUILD
  autoreconf -fiv
  ./configure --with-jpeg8 --prefix=/usr --mandir=/usr/share/man
  make
}
package() {
  cd $_svnmod-build
  make DESTDIR="$pkgdir/" install

  # provide jpegint.h as it is required by various software
  install -m644 jpegint.h $pkgdir/usr/include/

  # do not distributre libturbojpeg as it is unversioned
   rm $pkgdir/usr/lib/libturbojpeg.{so,a}
   rm $pkgdir/usr/include/turbojpeg.h
   rm $pkgdir/usr/bin/tjbench

  install -dm755 $pkgdir/usr/share/licenses/libjpeg-turbo-svn/
      ln -s ../../doc/libjpeg-turbo-svn/README \
  $pkgdir/usr/share/licenses/libjpeg-turbo-svn/README
      ln -s ../../doc/libjpeg-turbo-svn/README-turbo.txt \
  $pkgdir/usr/share/licenses/libjpeg-turbo-svn/README-turbo.txt
}

