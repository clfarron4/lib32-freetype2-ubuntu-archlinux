# Maintainer: 
# Contributor: Scott Bernynski <scott.bernynski@gmx.com>
# Contributor: Andrea Fagiani <andfagiani {at} gmail {dot} com>
# Contributor: Paul Bredbury <brebs@sent.com>
# Contributor: Biru Ionut <biru.ionut at gmail.com>

# Installation order:  freetype2-ubuntu fontconfig-ubuntu libxft-ubuntu cairo-ubuntu
_pkgbasename=freetype2-ubuntu
pkgname=lib32-$_pkgbasename
pkgver=2.4.12
_ubver=2.4.8-1
pkgrel=1
pkgdesc="TrueType font rendering library, with Ubuntu's LCD rendering patches (32-bit)"
arch=('x86_64')
url="https://launchpad.net/ubuntu/precise/+source/freetype"
license=('GPL')
depends=('lib32-zlib' 'freetype2-ubuntu')
makedepends=('gcc-multilib')
conflicts=('lib32-freetype2' 'lib32-freetype2-cleartype' 'lib32-freetype2-lcd')
provides=("lib32-freetype2=$pkgver")
options=('!libtool')
source=(http://downloads.sourceforge.net/sourceforge/freetype/freetype-${pkgver}.tar.bz2
        https://launchpad.net/ubuntu/precise/+source/freetype/$_ubver/+files/freetype_$_ubver.diff.gz
        freetype-2.2.1-enable-valid.patch)

md5sums=('3463102764315eb86c0d3c2e1f3ffb7d'
         '97a1f13bcf4c5e5a9fc32795fcc72c44'
         '214119610444c9b02766ccee5e220680')

build() {
  export CC="gcc -m32"
  export CXX="g++ -m32"
  export PKG_CONFIG_PATH="/usr/lib32/pkgconfig"

  cd ${srcdir}/freetype-${pkgver}

  # Patch from ubuntu
  patch -Np1 -i $srcdir/freetype_$_ubver.diff

  sed -e "s/-p[0-9]\|.*otvalid\.patch//g" \
      -i debian/patches-freetype/series

  sed -e 's/ src/ a\/src/g' \
      -e '/^Index.*fbase.c/,/EOF/d' \
      -i debian/patches-freetype/freetype-2.1.7-backwards.compat.patch

  for _f in $(cat debian/patches-freetype/series) ; do    
    patch -Np1 -i debian/patches-freetype/$_f    
  done

  # Patches from arch trunk
  patch -Np1 -i "${srcdir}/freetype-2.2.1-enable-valid.patch"

  ./configure --prefix=/usr --libdir=/usr/lib32
  make
}

package() {
  cd ${srcdir}/freetype-${pkgver}
  make DESTDIR=${pkgdir} install
  rm -rf $pkgdir/usr/{include,share,bin}
}
