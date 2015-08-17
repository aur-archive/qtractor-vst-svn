# Maintainer : prettyvanilla <prettyvanilla@lavabit.com>
# Contributor: Bernardo Barros <bernardobarros@gmail.com>

_pkgname=qtractor
pkgname=qtractor-vst-svn
pkgver=3210
pkgrel=2
pkgdesc="Audio/MIDI multitrack sequencer (with Linux-native VST support)"
arch=('i686' 'x86_64')
url="http://qtractor.sourceforge.net"
license=('GPL')
depends=('qt4' 'jack' 'suil' 'lilv' 'libmad' 'libsamplerate' 'rubberband' 'liblo')
makedepends=('subversion' 'ladspa' 'dssi' 'steinberg-vst')
provides=("$_pkgname" "$_pkgname-vst")
conflicts=("$_pkgname" "$_pkgname-vst")

_svntrunk='http://svn.code.sf.net/p/qtractor/code/trunk'
_svnmod='qtractor'

build() {
  cd "$srcdir"
  msg "Connecting to SVN server...."

  if [[ -d "$_svnmod/.svn" ]]; then
    (cd "$_svnmod" && svn up -r "$pkgver")
  else
    svn co "$_svntrunk" --config-dir ./ -r "$pkgver" "$_svnmod"
  fi

  msg "SVN checkout done or server timeout"
  msg "Starting build..."

  rm -rf "$srcdir/$_svnmod-build"
  svn export "$srcdir/$_svnmod" "$srcdir/$_svnmod-build"
  cd "$srcdir/$_svnmod-build"

  make -f Makefile.svn

  ./configure --prefix=/usr \
              --with-vst=/usr/include/vst

  make
}

package() {
  cd "$srcdir/$_svnmod-build"

  make DESTDIR="$pkgdir/" install
}

# vim:set ts=2 sw=2 et:
