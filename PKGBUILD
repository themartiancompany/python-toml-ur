# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: David Runge <dvzrv@archlinux.org>
# Maintainer: Daniel M. Capella <polyzen@archlinux.org>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Truocolo <truocolo@aol.com>

_py="python"
_pkg="toml"
pkgname="${_py}-${_pkg}"
pkgver=0.10.2
pkgrel=9
pkgdesc="A Python library for parsing and creating TOML"
arch=(
  'any'
)
_http="https://github.com"
_ns="uiri"
url="${_http}/${_ns}/${_pkg}"
license=(
  'MIT'
)
depends=(
  "${_py}"
)
makedepends=(
  "${_py}-build"
  "${_py}-installer"
  "${_py}-setuptools"
  "${_py}-wheel"
)
checkdepends=(
  "${_py}-numpy"
  "${_py}-pytest"
)
_pypi="https://files.pythonhosted.org/packages/source"
source=(
  "${_pypi}/${_pkg::1}/${_pkg}/${_pkg}-${pkgver}.tar.gz"
  "${pkgname}-0.10.1-install_type_hints.patch"
)
sha256sums=(
  'b3bda1d108d5dd99f4a20d24d9c348e91c4db7ab1b749200bded2f839ccbe68f'
  '7597ca115ba497797cf7c1330f5e93146ff5a924ef3bc331a8e77da12ee880cf'
)
b2sums=(
  'a613d5e86fc77279f2d7b130309627eed1cab3a65fe8647cb1dc3e58d09a9d58c078472638f06f45394b00c142bcb2bc75e9c0c649d58aade2f4f6bdab23f12e'
  '23449773cb0c7004d6c367c4cef666561e16fcd38166d17428a8192f6649bdfa76277f20bd4662f7571bc020fba6e5614e8f7dcf8502d18eb1e0ede6cb3ddde2')

prepare() {
  mv \
    -v \
    "${_pkg}-${pkgver}" \
    "${pkgname}-${pkgver}"
  cd \
    "${pkgname}-${pkgver}"
  patch \
    -Np1 \
    -i \
      ../"${pkgname}-0.10.1-install_type_hints.patch"
}

build() {
  cd \
    "${pkgname}-${pkgver}"
  "${_py}" \
    -m build \
    --wheel \
    --skip-dependency-check \
    --no-isolation
}

check() {
  cd \
    "${pkgname}-${pkgver}"
  export \
    PYTHONPATH="../build/lib/:${PYTHONPATH}"
  # disable useless tests
  pytest \
    -v tests \
    -k 'not test_invalid_tests and not test_valid_tests'
}

package() {
  cd \
    "${pkgname}-${pkgver}"
  "${_py}" \
    -m installer \
    --destdir="${pkgdir}" \
    dist/*.whl
  install \
    -vDm 644 \
    README.rst \
    -t "${pkgdir}/usr/share/doc/${pkgname}"

  # Symlink license file
  local \
    site_packages=$( \
      "${_py}" \
        -c \
	  "import site; print(site.getsitepackages()[0])")
  install \
    -d \
    "${pkgdir}/usr/share/licenses/${pkgname}"
  ln \
    -s \
    "${site_packages}/${_pkg}-${pkgver}.dist-info/LICENSE" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}

# vim:set sw=2 sts=-1 et:
