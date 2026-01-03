pkgname=node-gyp
pkgver=12.1.0
pkgrel=2
pkgdesc="Node.js native addon build tool"
arch=('x86_64')
url="https://github.com/nodejs/node-gyp"
license=('MIT')
depends=(
    'nodejs'
    'nodejs-nopt'
    'semver'
)
makedepends=(
    'git'
    'jq'
    'npm'
)
options=('!emptydirs')
source=(git+ssh://git@github.com/nodejs/node-gyp.git#tag=v${pkgver})
sha256sums=(e43a76b18eca7931d523bb3e26976c1e20c1f91b43ba243209930a38bb9f0151)

prepare() {
    cd ${pkgname}

    npm install
}

package() {
    cd ${pkgname}

    local mod_dir=/usr/lib/node_modules/${pkgname}

    install -vdm755 ${pkgdir}/{usr/bin,${mod_dir}}

    ln -s ${mod_dir}/bin/${pkgname}.js ${pkgdir}/usr/bin/${pkgname}

    npm prune --omit=dev

    mapfile -t mod_files < <(npm pack --dry-run --json | jq -r .[].files.[].path)
    cp --parents -a ${mod_files[@]} node_modules ${pkgdir}/${mod_dir}

    # Experimental dedup
    rm -r ${pkgdir}/${mod_dir}/node_modules/{,.bin/}{nopt,semver}
}
