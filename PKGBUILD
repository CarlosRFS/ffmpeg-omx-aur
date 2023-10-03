# Maintainer: Leuc

pkgname=ffmpeg-omx
pkgver=4.4.4
pkgrel=1
pkgdesc='Complete solution to record, convert and stream audio and video with OMX patches for StarFive Vision Five 2 SBC'
arch=('riscv64')
url=https://ffmpeg.org/
prefix=/opt/ffmpeg-omx
install=ffmpeg-omx.install
license=(GPL3)
depends=(
  alsa-lib
  aom
  bzip2
  fontconfig
  fribidi
  gmp
  gnutls
  gsm
  jack
  lame
  libass.so
  libavc1394
  libbluray.so
  libdav1d.so
  libdrm
  libfreetype.so
  libiec61883
  libmodplug
  libpulse
  librav1e.so
  libraw1394
  librsvg-2.so
  libsoxr
  libssh
  libtheora
  libva.so
  libva-drm.so
  libva-x11.so
  libvdpau
  libvidstab.so
  libvorbisenc.so
  libvorbis.so
  libvpx.so
  libwebp
  libx11
  libx264.so
  libx265.so
  libxcb
  libxext
  libxml2
  libxv
  libxvidcore.so
  libzimg.so
  opencore-amr
  openjpeg2
  opus
  sdl2
  speex
  srt
  svt-av1
  v4l-utils
  xz
  zlib
  libOMX_Core.so
)
makedepends=(
  amf-headers
  clang
  git
  patch
)
provides=(
  libavcodec.so
  libavdevice.so
  libavfilter.so
  libavformat.so
  libavutil.so
  libpostproc.so
  libswresample.so
  libswscale.so
)
#_tag=71fb6132637a2a430375c24afc381fff8b854fe7 #4.4.4
#_tag=0e15444aceca0e78f99f3d67758eb79d11b86599 #n5.0.3
_tag=ea3d24bbe3c58b171e55fe2151fc7ffaca3ab3d2 #n6.0
source=(
  "https://github.com/starfive-tech/Debian/raw/v0.9.0-engineering-release-wayland/multimedia/patch/ffmpeg/0002-avcodec-vaapi_h264-skip-decode-if-pic-has-no-slices.patch"
  "git+https://git.ffmpeg.org/ffmpeg.git#tag=${_tag}"
)
md5sums=(
  59c6098207203b8366e4bb35d899a32c
  SKIP
)

pkgver() {
  cd ffmpeg
  git describe --tags | sed 's/^n//'
}

prepare() {
  cp ../ported_patchs/*.patch ${srcdir}/
  cd ffmpeg
  #git cherry-pick -n 988f2e9eb063db7c1a678729f58aab6eba59a55b # fix nvenc on older gpus
  git cherry-pick -n 031f1561cd286596cdb374da32f8aa816ce3b135 # remove compressed_ten_bit_format
  for patchfile in ${srcdir}/*.patch; do
    patch -g0 -p1 --remove-empty-files --no-backup-if-mismatch --batch --forward --directory=${srcdir}/ffmpeg --input="${patchfile}"
  done
}

build() {
  cd ffmpeg

  ./configure \
    --prefix=$prefix \
    --disable-debug \
    --disable-doc \
    --enable-ffmpeg \
    --enable-ffprobe \
    --enable-ffplay \
    --disable-static \
    --disable-stripping \
    --enable-amf \
    --enable-cuda-llvm \
    --enable-lto \
    --enable-fontconfig \
    --enable-gmp \
    --enable-gnutls \
    --enable-gpl \
    --enable-libaom \
    --enable-libass \
    --enable-libbluray \
    --enable-libdav1d \
    --enable-libdrm \
    --enable-libfreetype \
    --enable-libfribidi \
    --enable-libgsm \
    --enable-libiec61883 \
    --enable-libjack \
    --enable-libmodplug \
    --enable-libmp3lame \
    --enable-libopencore_amrnb \
    --enable-libopencore_amrwb \
    --enable-libopenjpeg \
    --enable-libopus \
    --enable-libpulse \
    --enable-librav1e \
    --enable-librsvg \
    --enable-libsoxr \
    --enable-libspeex \
    --enable-libsrt \
    --enable-libssh \
    --enable-libsvtav1 \
    --enable-libtheora \
    --enable-libv4l2 \
    --enable-libvidstab \
    --enable-libvmaf \
    --enable-libvorbis \
    --enable-libvpx \
    --enable-libwebp \
    --enable-libx264 \
    --enable-libx265 \
    --enable-libxcb \
    --enable-libxml2 \
    --enable-libxvid \
    --enable-libzimg \
    --enable-shared \
    --enable-version3 \
    --enable-omx \
    --extra-cflags="-I/usr/include/omx-il" \
    --extra-libs="-lOMX_Core"

  make
  make tools/qt-faststart
}

package() {
  make DESTDIR="${pkgdir}" -C ffmpeg install

  install -dm0755 "$pkgdir/etc/ld.so.conf.d/"
  echo "$prefix/lib" > "$pkgdir/etc/ld.so.conf.d/ffmpeg-omx.conf"
}

# vim:set sw=2 ts=2 et:
