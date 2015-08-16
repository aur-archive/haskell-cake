# custom variables
_hkgname=cake
_licensefile=LICENSE

# PKGBUILD options/directives
pkgname=haskell-cake
pkgver=0.1.0
pkgrel=7
pkgdesc="A build-system library and driver"
url="http://hackage.haskell.org/package/${_hkgname}"
license=("GPL")
arch=('i686' 'x86_64')
makedepends=()
depends=("ghc=7.0.3-2"
         "haskell-encode=1.3.5-1"
         "haskell-binary=0.5.0.2-8"
         "haskell-cmdargs=0.7-18"
         "haskell-derive=2.5.4-4"
         "haskell-mtl=2.0.1.0-3.1"
         "haskell-puremd5=2.1.0.3-25"
         "haskell-regex-tdfa=1.1.8-18"
         "haskell-split=0.1.4.1-1")
options=('strip')
source=("http://hackage.haskell.org/packages/archive/${_hkgname}/${pkgver}/${_hkgname}-${pkgver}.tar.gz"
        "cabal.patch")
install="${pkgname}.install"
sha256sums=("93168fb2f5a6c539326ac50f582b2988f2cbec9352603598b0b1d3c78bdd1c3a"
            "f70a5c369f70d409974673562003c5b80153bd902ffca3bdcd585729e6b8e7a4")

# PKGBUILD functions
build() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    patch cake.cabal ${srcdir}/cabal.patch 
    runhaskell Setup configure -O -p --enable-split-objs --enable-shared \
        --prefix=/usr --docdir=/usr/share/doc/${pkgname} \
        --libsubdir=\$compiler/site-local/\$pkgid
    runhaskell Setup build
    runhaskell Setup haddock
    runhaskell Setup register --gen-script
    runhaskell Setup unregister --gen-script
    sed -i -r -e "s|ghc-pkg.*unregister[^ ]* |&'--force' |" unregister.sh
}

package() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    install -D -m744 register.sh   ${pkgdir}/usr/share/haskell/${pkgname}/register.sh
    install    -m744 unregister.sh ${pkgdir}/usr/share/haskell/${pkgname}/unregister.sh
    install -d -m755 ${pkgdir}/usr/share/doc/ghc/html/libraries
    ln -s /usr/share/doc/${pkgname}/html ${pkgdir}/usr/share/doc/ghc/html/libraries/${_hkgname}
    runhaskell Setup copy --destdir=${pkgdir}
    install -D -m644 ${_licensefile} ${pkgdir}/usr/share/licenses/${pkgname}/LICENSE
    rm -f ${pkgdir}/usr/share/doc/${pkgname}/${_licensefile}
}
