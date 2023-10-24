# This file is basically a full copy from https://github.com/michaellass/AUR jdk17-temurin 
# except here I'm using JDK 21, it's a recent release and I was eager to try it out, so 
# this package happens to be my first interaction Arch Linux packages creation :) 

_majorver=21
_minorver=0
_patchver=1
_buildver=12
_completever=21.0.1
pkgrel=1
pkgver=${_majorver}.${_minorver}.${_patchver}.u${_buildver}
_tag_ver=${_majorver}.${_minorver}.${_patchver}+${_buildver}
_tag_ver_short=${_majorver}.${_minorver}.${_patchver}+${_buildver%.*}

pkgname=jdk21-temurin
pkgdesc="Temurin ${_majorver} (OpenJDK ${_majorver} Java binaries by Adoptium, formerly AdoptOpenJDK)"
arch=('x86_64')
url='https://adoptium.net/'
license=('custom')

depends=('java-runtime-common>=3' 'java-environment-common' 'ca-certificates-utils' 'desktop-file-utils' 'libxrender' 'libxtst' 'alsa-lib')
optdepends=('gtk2: for the Gtk+ 2 look and feel'
            'gtk3: for the Gtk+ 3 look and feel')
provides=("java-runtime-headless=${_majorver}"
          "java-runtime-headless-openjdk=${_majorver}"
          "jre${_majorver}-openjdk-headless=${pkgver}"
          "jre-openjdk-headless=${pkgver}"
          "java-runtime=${_majorver}"
          "java-runtime-openjdk=${_majorver}"
          "jre${_majorver}-openjdk=${pkgver}"
          "jre-openjdk=${pkgver}"
          "java-environment=${_majorver}"
          "java-environment-openjdk=${_majorver}"
          "jdk${_majorver}-openjdk=${pkgver}"
          "jdk-openjdk=${pkgver}"
          "openjdk${_majorver}-src=${pkgver}"
          "openjdk-src=${pkgver}")
replaces=("jdk-adoptopenjdk")
backup=(etc/java-${_majorver}-temurin/logging.properties
        etc/java-${_majorver}-temurin/management/jmxremote.access
        etc/java-${_majorver}-temurin/management/jmxremote.password.template
        etc/java-${_majorver}-temurin/management/management.properties
        etc/java-${_majorver}-temurin/net.properties
        etc/java-${_majorver}-temurin/sdp/sdp.conf.template
        etc/java-${_majorver}-temurin/security/java.policy
        etc/java-${_majorver}-temurin/security/java.security
        etc/java-${_majorver}-temurin/security/policy/limited/default_local.policy
        etc/java-${_majorver}-temurin/security/policy/limited/default_US_export.policy
        etc/java-${_majorver}-temurin/security/policy/limited/exempt_local.policy
        etc/java-${_majorver}-temurin/security/policy/README.txt
        etc/java-${_majorver}-temurin/security/policy/unlimited/default_local.policy
        etc/java-${_majorver}-temurin/security/policy/unlimited/default_US_export.policy
        etc/java-${_majorver}-temurin/sound.properties)
install=install_jdk21-temurin.sh
options=(!strip)

source=(https://github.com/adoptium/temurin21-binaries/releases/download/jdk-21.0.1%2B12/OpenJDK21U-jdk_x64_linux_hotspot_21.0.1_12.tar.gz
        freedesktop-java.desktop
        freedesktop-jconsole.desktop
        freedesktop-jshell.desktop)
sha256sums=('1a6fa8abda4c5caed915cfbeeb176e7fbd12eb6b222f26e290ee45808b529aa1'
            '23ced5efdb3381e6318af1acb343c714b2d9aacdb4666fee05dc58cd254b2d07'
            'ef7f6465272608a35615665f8e4b0837b43b4db7bf22f8eb7cecdd6170b5b2ee'
            '166b128e025404e7b4048c5b3c958e080a13a98f84b6faa9cfe11530f553e287')

_jvmdir=/usr/lib/jvm/java-${_majorver}-temurin
_jdkdir=jdk-${_tag_ver}

package() {

  install -dm 755 "${pkgdir}${_jvmdir}"
  cp -a "${srcdir}/${_jdkdir}"/* "${pkgdir}${_jvmdir}"

  cd "${pkgdir}${_jvmdir}"

  # Conf
  install -dm 755 "${pkgdir}/etc"
  mv conf "${pkgdir}/etc/java-${_majorver}-temurin"
  ln -sf /etc/java-${_majorver}-temurin conf

  # Legal
  install -dm 755 "${pkgdir}/usr/share/licenses"
  mv legal "${pkgdir}/usr/share/licenses/${pkgname}"
  ln -sf /usr/share/licenses/${pkgname} legal

  # Man pages
  for f in man/man1/*; do
    install -Dm 644 "${f}" "${pkgdir}/usr/share/${f/\.1/-temurin${_majorver}.1}"
  done
  rm -rf man
  ln -sf /usr/share/man man

  # Link JKS keystore from ca-certificates-utils
  rm -f lib/security/cacerts
  ln -sf /etc/ssl/certs/java/cacerts lib/security/cacerts

  # Desktop files
  for f in jconsole java jshell; do
    install -Dm 644 \
      "${srcdir}/freedesktop-${f}.desktop" \
      "${pkgdir}/usr/share/applications/${f}-${pkgname}.desktop"
  done

}
