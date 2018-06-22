# Maintainer: Andrew Sun <adsun701@gmail.com>
# Contributor: Stas Elensky <stas-at-flexsys-dot-com-dot-ua>

pkgname=mingw-w64-libmodbus
pkgver=3.1.4
pkgrel=1
pkgdesc="A Modbus library for Linux, Mac OS X, FreeBSD, QNX and Win32 (mingw-w64)"
arch=('any')
url="http://libmodbus.org/"
license=('LGPL')
depends=('mingw-w64-crt')
makedepends=('mingw-w64-configure')
options=(!strip !buildflags staticlibs)
_pkgfqn="libmodbus-${pkgver}"
source=("http://libmodbus.org/releases/libmodbus-${pkgver}.tar.gz")
md5sums=('b1a8fd3a40d2db4de51fb0cbcb201806')

_architectures="i686-w64-mingw32 x86_64-w64-mingw32"

prepare() {
  cd "${srcdir}/${_pkgfqn}"
  autoreconf -fiv
}

build() {
  unset CFLAGS
  unset LDFLAGS
  export ac_cv_func_malloc_0_nonnull=yes

  cd "$srcdir/$_pkgfqn"

  # skip tests
  sed -i 's/ tests//' Makefile.am

  for _arch in ${_architectures}; do
    mkdir -p build-${_arch}
    pushd build-${_arch}
    ${_arch}-configure --without-documentation ..
    make
    popd
  done
}

package() {
  for _arch in ${_architectures}; do
    cd "$srcdir/$_pkgfqn/build-${_arch}"
    make DESTDIR="$pkgdir" install

    find "$pkgdir" -name '*.dll' -exec ${_arch}-strip --strip-unneeded {} \;
    find "$pkgdir" -name '*.dll' -o -name '*.a' -exec ${_arch}-strip -g {} \;
  done
}

# vim:set ts=2 sw=2 et:
