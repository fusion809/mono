# $Id$
# Maintainer: Daniel Isenmann <daniel@archlinux.org>
# Contributor: Brice Carpentier <brice@dlfp.org>

pkgname=mono
pkgver=4.6.0.245
_pkgver=4.6.0
pkgrel=1
pkgdesc="Free implementation of the .NET platform including runtime and compiler"
arch=(i686 x86_64)
license=('GPL' 'LGPL2.1' 'MPL' 'custom:MITX11')
url="http://www.mono-project.com/"
depends=('zlib' 'libgdiplus>=4.2' 'sh' 'python' 'ca-certificates')
provides=('monodoc')
conflicts=('monodoc')
install="${pkgname}.install"
source=(http://download.mono-project.com/sources/mono/${pkgname}-${pkgver}.tar.bz2
        mono.binfmt.d
	mono_context.patch)
md5sums=('0567cfc1a50e3260f968cd70ecd7495c'
         'b9ef8a65fea497acf176cca16c1e2402'
         '9325e50a3fde354229c507801622b64b')

build() {
  cd "${srcdir}"/${pkgname}-${_pkgver}
	
  # build mono
  ./configure \
    --prefix=/usr \
    --sysconfdir=/etc \
    --bindir=/usr/bin \
    --sbindir=/usr/bin \
    --with-mcs-docs=no 
  make

  # build jay
  cd "${srcdir}"/${pkgname}-${_pkgver}/mcs/jay
  make
}

package() {
  cd "${srcdir}"/${pkgname}-${_pkgver}
  make DESTDIR="${pkgdir}" install

  # install jay
  pushd "${srcdir}"/${pkgname}-${_pkgver}/mcs/jay
  make DESTDIR="${pkgdir}" prefix=/usr INSTALL=../../install-sh install
  popd

  # install binfmt conf file and pathes
  install -D -m644 "${srcdir}"/mono.binfmt.d "${pkgdir}"/usr/lib/binfmt.d/mono.conf

  #install license
  mkdir -p "${pkgdir}"/usr/share/licenses/${pkgname}
  install -m644 mcs/COPYING "${pkgdir}"/usr/share/licenses/${pkgname}/

  #fix .pc file to be able to request mono on what it depends, fixes #go-oo build
  sed -i -e "s:#Requires:Requires:" "${pkgdir}"/usr/lib/pkgconfig/mono.pc
  sed -i -e "s:/2.0/:/4.5/:g" "${pkgdir}"/usr/lib/pkgconfig/mono-nunit.pc
}
