# Maintainer: Hera Chamorro (hera@hera.wtf)

pkgname='herix-git'
_pkgname='helix'
pkgver=88bc52a
pkgrel=1
pkgdesc="Hera's fork of the Helix text editor."
url="https://helix-editor.com"
_git="http://github.com/herabit/${_pkgname}.git"
_branch="master"
arch=('x86_64')
makedepends=('cargo')
depends=()
provides=('hx' 'helix')
conflicts=('helix' 'helix-git')
source=("${_pkgname}::git+${_git}#branch=${_branch}")
sha256sums=('SKIP')

_bin='hx'
_alt_bin='helix'
_lib_path="/usr/lib/${_pkgname}"
_rt_path="${_lib_path}/runtime"


pkgver() {
    cd "${_pkgname}"
    git describe --always --long --abbrev=7 | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
    cat > "$_bin" << EOF
#!/usr/bin/env sh
HELIX_RUNTIME=${_rt_path} exec ${_lib_path}/${_bin} "\$@"
EOF
    chmod +x "$_bin"

    cd "${_pkgname}"
    git fetch --all
    git submodule init
    git submodule update --force --init --recursive --depth 1 --jobs 8

}

build() {
    cd "${_pkgname}"
    cargo build --release --locked
}

check() {
    cd "${_pkgname}"
    cargo test --all-features
}

package() {
    cd "${_pkgname}"
    mkdir -p "${pkgdir}${_lib_path}"
    rm -r  "runtime/grammars/sources" 
    cp -r "runtime" "${pkgdir}${_lib_path}"
    install -Dm 0755 "target/release/${_bin}" "${pkgdir}${_lib_path}/${_bin}"
    install -Dm 0644 "LICENSE" "${pkgdir}/usr/share/licenses/${_pkgname}/LICENSE"
    install -Dm 0777 "${srcdir}/${_bin}" "${pkgdir}/usr/bin/${_bin}"
    install -Dm 0644 "contrib/Helix.desktop" "${pkgdir}/usr/share/applications/Helix.desktop"
    install -Dm 0644 "contrib/Helix.appdata.xml" "${pkgdir}/usr/share/appdata/Helix.appdata.xml"
    install -Dm 0644 "contrib/helix.png" "${pkgdir}/usr/share/icons/Helix.png"
    install -Dm 0644 "contrib/completion/hx.zsh" "${pkgdir}/usr/share/zsh/site-functions/_hx"
    install -Dm 0644 "contrib/completion/hx.bash" "${pkgdir}/usr/share/bash-completion/completions/hx.bash"
    install -Dm 0644 "contrib/completion/hx.fish" "${pkgdir}/usr/share/fish/vendor_completions.d/hx.fish"

    ln -s "${pkgdir}/usr/bin/${_bin}" "${pkgdir}/usr/bin/${_alt_bin}"
}
