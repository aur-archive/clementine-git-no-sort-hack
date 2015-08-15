# Original-Maintainer: Adria Arrufat <swiftscythe@gmail.com>
# Contributor: Ike Devolder <ike.devolder@gmail.com>
# Contributor: Daniel Hill <daniel.hill@orcon.net.nz>
# Contributor: Xiao-Long Chen <chenxiaolong@cxl.epac.to>
# Maintainer: Andreas Eisele <a.e@lupuz.de>

pkgname=clementine-git-no-sort-hack
_realname=clementine-player
pkgver=20130304
pkgrel=1
pkgdesc="A modern music player and library organiser and a port of Amarok 1.4, with some features rewritten to take advantage of Qt4. - Track Sorting disabled"
arch=('i686' 'x86_64')
url="http://code.google.com/p/clementine-player/"
license=('GPL')
depends=('gstreamer0.10-base' 'taglib' 'glew' 'liblastfm' 'libgpod'
         'libmtp' 'libplist' 'hicolor-icon-theme' 'qt4' 'sparsehash' 
         'qjson' 'libcdio' 'protobuf' 'qca' 'qca-ossl' 'chromaprint')
optdepends=('gstreamer0.10-base-plugins: for more open formats'
            'gstreamer0.10-good-plugins: for use with "Good" plugin libraries'
            'gstreamer0.10-bad-plugins: for use with "Bad" plugin libraries'
            'gstreamer0.10-ugly-plugins: for use with "Ugly" plugin libraries'
            )
makedepends=('git' 'boost' 'cmake' 'mesa')
# Uncomment next lines to enable more features
#makedepends+=('libspotify' 'libgpod' 'libimobiledevice')
#optdepends+=(
            #'libspotify: for Spotify support'
            #'libgpod: for iPod support'
            #'libimobiledevice: for iPhone and iPod Touch support'
            #)

provides=('clementine')
conflicts=('clementine' 'clementine-svn' 'clementine-git')
replaces=('clementine' 'clementine-svn')
source=(no-sort.patch)
sha512sums=('25ad6010e869328907c675bdc9f010fefd9ee9057ea19ebe0fa93b496b1c49a5d6ed4e75ae81fd08465d7eedeb4d2c1515d5ae30ca8b9863883e47ab845c2c49')
install=clementine.install

_gitroot="https://code.google.com/p/${_realname}"
_gitname=${_realname}

build() {
  cd "${srcdir}"
  msg "Connecting to GIT server...."

  if [[ -d "${_gitname}" ]]; then
    cd "${_gitname}" && git pull origin
    msg "The local files are updated."
  else
    git clone "${_gitroot}" "${_gitname}"
  fi

  msg "GIT checkout done or server timeout"
  msg "Starting build..."

  rm -rf "${srcdir}/${_gitname}-build"
  git clone "${srcdir}/${_gitname}" "${srcdir}/${_gitname}-build"
  cd "${srcdir}/${_gitname}-build"

  patch -p1 < ${srcdir}/no-sort.patch
  cmake . -DCMAKE_INSTALL_PREFIX=/usr -DBUILDBOT_REVISION=${pkgver} \
    -DENABLE_SPOTIFY=OFF -DENABLE_SPOTIFY_BLOB=OFF #-DENABLE_GIO=OFF
  make
}

package() {
  cd "${srcdir}/${_gitname}-build"

  #sed -i -e '/"\/.*version[[:digit:]]*[-]*[[:digit:]]*bit/ s//"${CMAKE_INSTALL_PREFIX}\/bin/' -e '/clementine-spotifyblob/ s/$ENV{DESTDIR}//g' ext/clementine-spotifyblob/cmake_install.cmake
  make DESTDIR="${pkgdir}" install
}

# vim:set ts=2 sw=2 et:
