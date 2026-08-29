# SPDX-License-Identifier: AGPL-3.0

#    -----------------------------------------------------
#    Copyright © 2024, 2025, 2026  Pellegrino Prevete
#
#    All rights reserved
#    -----------------------------------------------------
#
#    This program is free software: you can redistribute
#    it and/or modify it under the terms of the
#    GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of
#    the License, or (at your option) any later version.
#
#    This program is distributed in the hope that it
#    will be useful, but WITHOUT ANY WARRANTY;
#    without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
#    See the GNU Affero General Public License for
#    more details.
#
#    You should have received a copy of the
#    GNU Affero General Public License
#    along with this program.
#    If not, see <https://www.gnu.org/licenses/>.

# Maintainers:
#   Truocolo
#     <truocolo@aol.com>
#     <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
#   Pellegrino Prevete (dvorak)
#     <pellegrinoprevete@gmail.com>
#     <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Contributors:
#   Mark Wagie
#     <mark dot wagie at proton dot me>
#   hexchain
#     <i@hexchain.org>

if [[ ! -v "_build" ]]; then
  _build="true"
fi
_py=python
_pyver="$(
  "${_py}" \
    -V | \
    awk \
      '{print $2}' || \
  true)"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$((
  "${_pyminver}" + 1))"
_pkg=requirements-parser
_setuptools="true"
pkgname="${_py}-${_pkg}"
pkgver=0.13.1
_commit="138dc9fdbd98d8cd74c2fbc8fa3d4e2a1047face"
pkgrel=5
pkgdesc="A Pip requirements file parser."
arch=('any')
_http="https://github.com"
_ns="madpah"
_ns="themartiancompany"
url="${_http}/${_ns}/${_pkg}"
license=(
  'Apache-2.0'
)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
  "${_py}-packaging"
)
makedepends=(
  "${_py}-build"
  "${_py}-installer"
  "${_py}-poetry-core"
  "${_py}-wheel"
)
checkdepends=(
  "${_py}-pytest"
)
if [[ "${_ns}" == "madpah" ]]; then
  _tag_name="tag"
  _tag="${pkgver}"
  _sum='0503638c426185abd5647df49e875dbb0051a5a295688321c69b74b373dc6b53'
elif [[ "${_ns}" == "themartiancompany" ]]; then
  _tag_name="commit"
  _tag="${_commit}"
  _sum="boh"
fi
_url="${url}"
if [[ "${_tag_name}" == "tag" ]]; then
  _archive_format="tar.gz"
  _url="${_url}/archive/refs/tags/v${_tag}.tar.gz"
elif [[ "${_tag_name}" == "commit" ]]; then
  _archive_format="zip"
  _uri="${_url}/archive/${_commit}.${_archive_format}"
fi
_tarname="${_pkg}-${_tag}"
_tarfile="${_tarname}.${_archive_format}"
_src="${_tarfile}::${_uri}"
source=(
  "${_src}"
)
sha256sums=(
  "${_sum}"
)

build() {
  cd \
    "${_tarname}"
  GIT_DIR='.' \
  "${_py}" \
    -m \
      "build" \
      --wheel \
      --no-isolation
}

check() {
  cd \
    "${_tarname}"
  "${_py}" \
    -m \
      "venv" \
    --clear \
    --without-pip \
    --system-site-packages \
    "test-env"
  "test-env/bin/${_py}" \
    -m \
      "installer" \
    "dist/"*".whl"
  "test-env/bin/${_py}" \
    -I \
    -m \
      "pytest"
}

package() {
  cd \
    "${_tarname}"
  "${_py}" \
    -m \
      "installer" \
      --destdir="${pkgdir}" \
      "dist/"*".whl"
}
