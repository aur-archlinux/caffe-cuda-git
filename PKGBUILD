# Maintainer : Daniel Bermond <dbermond@archlinux.org>

_author=AlexandrParkhomenko
_pkgname=caffe-cuda
pkgname=${_pkgname}-git
pkgver=1.0.r1.g7efd0f2
pkgrel=1
pkgdesc='A deep learning framework made with expression, speed, and modularity in mind (with cuda support, git version)'
arch=(
# 'any'
  'x86_64'
)

url='https://caffe.berkeleyvision.org/'
license=('BSD')
depends=('openblas' 'lapack' 'boost-libs' 'protobuf' 'google-glog' 'gflags'
         'hdf5' 'opencv' 'leveldb' 'lmdb' 'python' 'python-numpy' 'python-pandas'
         'cuda' 'nccl')
optdepends=(
    # official repositories:
        'cython' 'python-scipy' 'python-matplotlib' 'ipython' 'python-h5py'
        'python-networkx' 'python-nose' 'python-dateutil' 'python-protobuf'
        'python-gflags' 'python-yaml' 'python-pillow' 'python-six'
    # AUR:
        'python-leveldb' 'python-scikit-image' 'python-pydotplus'
    # NOTE:
    # python-pydotplus (or python-pydot) is required by python executable 'draw_net.py'
    # https://github.com/BVLC/caffe/blob/9b891540183ddc834a02b2bd81b31afae71b2153/python/caffe/draw.py#L7-L22
)
makedepends=('git' 'boost'
# 'doxygen' 'texlive-core' 'texlive-latexextra' 'ghostscript'
)
provides=('caffe' 'caffe-cuda')
conflicts=('caffe')

source=("git://github.com/$_author/$_pkgname")
sha256sums=('SKIP')
# edit Makefile.config for your GPU

pkgver() {
  cd "$srcdir/$_pkgname"
  git describe --tags --long | sed -r 's/^v//;s/-RC/RC/;s/([^-]*-g)/r\1/;s/-/./g'
  #git -C caffe describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
  #git rev-list --count HEAD
}

build() {
  cd "$srcdir/$_pkgname"
  pwd
  make all pycaffe test
  rm -rf  doxygen
# make -C docs
  make distribute
}

check() {
  cd "$srcdir/$_pkgname"
  make runtest
}

package() {
  cd "$srcdir/$_pkgname"
  local _pyver
  _pyver="$(python -c 'import sys; print("%s.%s" %sys.version_info[0:2])')"
    
#  mkdir -p "$pkgdir"/usr/{bin,include,lib/python"$_pyver"/site-packages,share/doc}
  mkdir -p "$pkgdir"/usr/{bin,include,lib/python"$_pyver"/site-packages}
    
  # binaries
  install -m755 distribute/bin/* "${pkgdir}/usr/bin"
    
  # library
  cp -a distribute/lib/libcaffe.so* "${pkgdir}/usr/lib"
  chmod 755 "${pkgdir}/usr/lib"/libcaffe.so.*.*.*
    
  # headers
  cp -a distribute/include "${pkgdir}/usr"
    
  # python
  install -m755 distribute/python/*.py "${pkgdir}/usr/bin"
  cp -a distribute/python/caffe "${pkgdir}/usr/lib/python${_pyver}/site-packages"
    
  # proto
  install -D -m644 distribute/proto/caffe.proto -t "${pkgdir}/usr/share/caffe"
    
  # docs
# cp -a doxygen/html "${pkgdir}/usr/share/doc/${pkgname}"
    
  # license
  install -D -m644 LICENSE -t "${pkgdir}/usr/share/licenses/${pkgname}"
}
