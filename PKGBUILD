# Maintainer: Excitable Snowball <excitablesnowball@gmail.com>
pkgname=papilio-loader
pkgver=2.6
pkgrel=1
pkgdesc="GUI tool for loading Papilio bitstreams."
arch=("i686" "x86_64")
url="https://github.com/GadgetFactory/Papilio-Loader"
license=("GPL2" "LGPL2.1")
depends=('papilio-prog>=2.6' 'srecord' 'java-environment=7')
optdepends=('papilio-udev')
makedepends=('jdk7-openjdk')
install='papilio-loader.install'
source=("https://github.com/GadgetFactory/Papilio-Loader/archive/V${pkgver}.tar.gz"
        'papilio-loader.sh')
md5sums=('d8e3d384bac69a84d3e41ec23f41f0c6'
         '86576141eb8156c3369bace1734e1156')

# Xilinx ISE location
_isever=14.7
_isedir="/opt/Xilinx/$_isever/ISE_DS/ISE/bin"
if test "$CARCH" == x86_64; then
  _data2mem="$_isedir/lin64/data2mem"
else
  _data2mem="$_isedir/lin/data2mem"
fi

build() {
  cd "$srcdir/Papilio-Loader-$pkgver/Java-GUI"
  ./build.sh
}

package() {
  cd "$srcdir/Papilio-Loader-$pkgver/Java-GUI"

  # Install main program
  mkdir -p "$pkgdir/opt/papilio-loader"
  cp papilio-loader.jar "$pkgdir/opt/papilio-loader"
  install "$srcdir/papilio-loader.sh" "$pkgdir/opt/papilio-loader"
  cp -r images "$pkgdir/opt/papilio-loader"
  cp -r help "$pkgdir/opt/papilio-loader"

  # Install binaries and set up links to dependencies
  mkdir -p "$pkgdir/opt/papilio-loader/programmer/linux32"
  cp programmer/*.bit "$pkgdir/opt/papilio-loader/programmer"
  cp programmer/linux32/devlist.txt "$pkgdir/opt/papilio-loader/programmer/linux32"
  ln -s /usr/bin/srec_cat "$pkgdir/opt/papilio-loader/programmer/linux32/srec_cat"
  ln -s /usr/bin/papilio-prog "$pkgdir/opt/papilio-loader/programmer/linux32/papilio-prog"
  if [ -f $_data2mem ]; then
    ln -s $_data2mem "$pkgdir/opt/papilio-loader/programmer/linux32/data2mem"
  fi

  # Link in /usr/bin for convenience
  mkdir -p "$pkgdir/usr/bin"
  ln -s /opt/papilio-loader/papilio-loader.sh "$pkgdir/usr/bin/papilio-loader-gui"
}
