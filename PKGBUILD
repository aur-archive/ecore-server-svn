# Maintainer: kuri <sysegv@gmail.com>
# Contributor: chefpeyo <pierre-olivier.huguet@asp64.com>

pkgname=ecore-server-svn
pkgver=78937
pkgrel=1
pkgdesc="Ecore is a clean and tiny event loop library. This package will only build modules core, con, file and ipc. This is best suited for CLI apps, deamons..."
arch=('i686' 'x86_64')
url="http://www.enlightenment.org"
license=('LGPL2')
depends=('eina-svn')
makedepends=('subversion')
conflits=('ecore' 'ecore-svn')
provides=('ecore' 'ecore-svn')
options=('!libtool')
source=()
md5sums=()

_svntrunk="http://svn.enlightenment.org/svn/e/trunk/ecore"
_svnmod="ecore"

build() {
  cd $srcdir

  if [ $NOEXTRACT -eq 0 ]; then
    msg "Connecting to $_svntrunk SVN server...."
    if [ -d $_svnmod/.svn ]; then
      (cd $_svnmod && svn up -r $pkgver)
    else
      svn co $_svntrunk --config-dir ./ -r $pkgver $_svnmod
    fi

    msg "SVN checkout done or server timeout"
    msg "Starting make..."

  fi
  cp -r $_svnmod $_svnmod-build
  cd $_svnmod-build

  ./autogen.sh --prefix=/usr                                                   \
               --enable-ecore-ipc --enable-ecore-con --enable-ecore-file       \
               --disable-ecore-input --disable-ecore-input-evas                \
               --disable-ecore-x --disable-ecore-evas --disable-glib           \
               --disable-tslib --disable-xim --disable-ecore-x-composite       \
               --disable-ecore-x-damage --disable-ecore-x-dpms                 \
               --disable-ecore-x-randr --disable-ecore-x-render                \
               --disable-ecore-x-screensaver --disable-ecore-x-shape           \
               --enable-ecore-x-gesture --disable-ecore-x-sync                 \
               --disable-ecore-x-xfixes --disable-ecore-x-xinerama             \
               --disable-ecore-x-xprint --disable-ecore-x-xtest                \
               --disable-ecore-x-cursor --disable-ecore-x-input                \
               --disable-ecore-x-dri --disable-ecore-imf --disable-ecore-fb
  make || return 1
  make DESTDIR=$pkgdir install || return 1

# install license files
  install -Dm644 $srcdir/$_svnmod-build/COPYING \
  	$pkgdir/usr/share/licenses/$pkgname/COPYING

  rm -r $startdir/src/$_svnmod-build
}
