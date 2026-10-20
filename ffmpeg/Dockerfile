# Static ffmpeg and ffprobe in an empty image, to copy into other images:
#   COPY --from=flat/ffmpeg /ffmpeg /ffprobe /usr/local/bin/
#
# Built on Alpine (musl) so the binaries are fully static. Network protocols are disabled.
# Sources are pinned by version and sha256 (x264 by commit).
ARG ALPINE_VERSION=alpine:3.24.2
FROM $ALPINE_VERSION AS builder

RUN apk add --no-cache \
  bash \
  build-base \
  bzip2-dev bzip2-static \
  cmake \
  coreutils \
  diffutils \
  file \
  git \
  linux-headers \
  meson \
  nasm \
  ninja \
  perl \
  pkgconfig \
  tar \
  wget \
  xz \
  zlib-dev zlib-static

# -static-libgcc keeps gcc_s from being linked as an implicit shared library. The rest matches
# FFmpeg's hardened toolchain.
ARG CFLAGS="-O3 -static-libgcc -fno-strict-overflow -fstack-protector-all -fPIC"
ARG CXXFLAGS="-O3 -static-libgcc -fno-strict-overflow -fstack-protector-all -fPIC"
ARG LDFLAGS="-Wl,-z,relro,-z,now"
ARG WGET_OPTS="--tries=10 --connect-timeout=10 --retry-connrefused --retry-on-host-error --retry-on-http-error=429,500,502,503,504"
ARG TAR_OPTS="--no-same-owner --extract --file"

ARG AOM_VERSION=3.15.2
ARG AOM_URL="https://storage.googleapis.com/aom-releases/libaom-$AOM_VERSION.tar.gz"
ARG AOM_SHA256=67bb54b245f33ed98600e08269e6139986e48114e14042474ddd8885801dfddc
RUN \
  wget $WGET_OPTS -O aom.tar.gz "$AOM_URL" && \
  echo "$AOM_SHA256  aom.tar.gz" | sha256sum -c - && \
  tar $TAR_OPTS aom.tar.gz && cd libaom-* && \
  mkdir build_tmp && cd build_tmp && \
  cmake \
    -G"Unix Makefiles" \
    -DCMAKE_BUILD_TYPE=Release \
    -DBUILD_SHARED_LIBS=OFF \
    -DENABLE_APPS=NO \
    -DENABLE_EXAMPLES=NO \
    -DENABLE_DOCS=NO \
    -DENABLE_TESTS=NO \
    -DENABLE_TOOLS=NO \
    -DCONFIG_LIBYUV=0 \
    -DENABLE_NASM=ON \
    -DCMAKE_INSTALL_LIBDIR=lib \
    .. && \
  make -j$(nproc) install

ARG DAV1D_VERSION=1.5.4
ARG DAV1D_URL="https://download.videolan.org/pub/videolan/dav1d/$DAV1D_VERSION/dav1d-$DAV1D_VERSION.tar.xz"
ARG DAV1D_SHA256=686616b7c69eb88d44459391ab25cac13b6647a3b288835c5784e71c1514a5c5
RUN \
  wget $WGET_OPTS -O dav1d.tar.xz "$DAV1D_URL" && \
  echo "$DAV1D_SHA256  dav1d.tar.xz" | sha256sum -c - && \
  tar $TAR_OPTS dav1d.tar.xz && cd dav1d-* && \
  meson setup build \
    -Dbuildtype=release \
    -Ddefault_library=static && \
  ninja -j$(nproc) -C build install

ARG MP3LAME_VERSION=4.0
ARG MP3LAME_URL="https://sourceforge.net/projects/lame/files/lame/$MP3LAME_VERSION/lame-$MP3LAME_VERSION.tar.gz/download"
ARG MP3LAME_SHA256=3df5124d5ad3a98312ffd7ba6a9b36230e4f8a3e66d3ce0f425e336c32d216eb
RUN \
  wget $WGET_OPTS -O lame.tar.gz "$MP3LAME_URL" && \
  echo "$MP3LAME_SHA256  lame.tar.gz" | sha256sum -c - && \
  tar $TAR_OPTS lame.tar.gz && cd lame-* && \
  ./configure \
    --disable-shared \
    --enable-static \
    --enable-nasm \
    --disable-gtktest \
    --disable-cpml \
    --disable-decoder \
    --disable-frontend && \
  make -j$(nproc) install

ARG OPUS_VERSION=1.6.1
ARG OPUS_URL="https://downloads.xiph.org/releases/opus/opus-$OPUS_VERSION.tar.gz"
ARG OPUS_SHA256=6ffcb593207be92584df15b32466ed64bbec99109f007c82205f0194572411a1
RUN \
  wget $WGET_OPTS -O opus.tar.gz "$OPUS_URL" && \
  echo "$OPUS_SHA256  opus.tar.gz" | sha256sum -c - && \
  tar $TAR_OPTS opus.tar.gz && cd opus-* && \
  ./configure \
    --disable-shared \
    --enable-static \
    --disable-extra-programs \
    --disable-doc && \
  make -j$(nproc) install

ARG OGG_VERSION=1.3.6
ARG OGG_URL="https://downloads.xiph.org/releases/ogg/libogg-$OGG_VERSION.tar.gz"
ARG OGG_SHA256=83e6704730683d004d20e21b8f7f55dcb3383cdf84c0daedf30bde175f774638
RUN \
  wget $WGET_OPTS -O libogg.tar.gz "$OGG_URL" && \
  echo "$OGG_SHA256  libogg.tar.gz" | sha256sum -c - && \
  tar $TAR_OPTS libogg.tar.gz && cd libogg-* && \
  ./configure \
    --disable-shared \
    --enable-static && \
  make -j$(nproc) install

ARG VORBIS_VERSION=1.3.7
ARG VORBIS_URL="https://downloads.xiph.org/releases/vorbis/libvorbis-$VORBIS_VERSION.tar.gz"
ARG VORBIS_SHA256=0e982409a9c3fc82ee06e08205b1355e5c6aa4c36bca58146ef399621b0ce5ab
RUN \
  wget $WGET_OPTS -O libvorbis.tar.gz "$VORBIS_URL" && \
  echo "$VORBIS_SHA256  libvorbis.tar.gz" | sha256sum -c - && \
  tar $TAR_OPTS libvorbis.tar.gz && cd libvorbis-* && \
  ./configure \
    --disable-shared \
    --enable-static \
    --disable-oggtest && \
  make -j$(nproc) install

ARG VPX_VERSION=1.17.0
ARG VPX_URL="https://github.com/webmproject/libvpx/archive/v$VPX_VERSION.tar.gz"
ARG VPX_SHA256=1020f184046187baa2985dbde38e0691f49c44088bca7a1842b0236c6081dc0a
RUN \
  wget $WGET_OPTS -O libvpx.tar.gz "$VPX_URL" && \
  echo "$VPX_SHA256  libvpx.tar.gz" | sha256sum -c - && \
  tar $TAR_OPTS libvpx.tar.gz && cd libvpx-* && \
  ./configure \
    --enable-static \
    --enable-vp9-highbitdepth \
    --disable-shared \
    --disable-unit-tests \
    --disable-examples && \
  make -j$(nproc) install

# x264 has a stable branch and no release tarballs: pinned by commit.
ARG X264_URL="https://code.videolan.org/videolan/x264.git"
ARG X264_VERSION=b35605ace3ddf7c1a5d67a2eb553f034aef41d55
RUN \
  git clone "$X264_URL" && cd x264 && \
  git checkout $X264_VERSION && \
  ./configure \
    --enable-pic \
    --enable-static \
    --disable-cli && \
  make -j$(nproc) install

# multilib.sh builds 8, 10 and 12 bit into one library. The sed lines pass CMAKEFLAGS through and
# keep the static build.
ARG X265_VERSION=4.3
ARG X265_URL="https://github.com/Multicorewareinc/x265/releases/download/$X265_VERSION/x265_$X265_VERSION.tar.gz"
ARG X265_SHA256=83c53e4c8bbb8f1e33ed59e10a7d621d1d7801ca853910c3eb41f038b8ffb121
RUN \
  wget $WGET_OPTS -O x265.tar.gz "$X265_URL" && \
  echo "$X265_SHA256  x265.tar.gz" | sha256sum -c - && \
  tar $TAR_OPTS x265.tar.gz && cd x265_*/build/linux && \
  sed -i '/^cmake / s/$/ -G "Unix Makefiles" ${CMAKEFLAGS}/' ./multilib.sh && \
  sed -i 's/ -DENABLE_SHARED=OFF//g' ./multilib.sh && \
  MAKEFLAGS="-j$(nproc)" \
  CMAKEFLAGS="-DENABLE_SHARED=OFF -DCMAKE_BUILD_TYPE=Release" \
  ./multilib.sh && \
  make -C 8bit -j$(nproc) install

# The sed turns --toolchain=hardened's -pie into -static-pie. The stack-size flag raises musl's
# 128 KB default thread stack to glibc's 2 MB: libaom segfaults without it.
ARG FFMPEG_VERSION=9.0.2
ARG FFMPEG_URL="https://ffmpeg.org/releases/ffmpeg-$FFMPEG_VERSION.tar.bz2"
ARG FFMPEG_SHA256=ee80a25a5621df84498cef46917be87eab3b4f01951e85c2c7cbe5c94914ee70
RUN \
  wget $WGET_OPTS -O ffmpeg.tar.bz2 "$FFMPEG_URL" && \
  echo "$FFMPEG_SHA256  ffmpeg.tar.bz2" | sha256sum -c - && \
  tar $TAR_OPTS ffmpeg.tar.bz2 && cd ffmpeg-* && \
  sed -i 's/add_ldexeflags -fPIE -pie/add_ldexeflags -fPIE -static-pie/' configure && \
  ./configure \
    --pkg-config-flags="--static" \
    --extra-ldflags="-Wl,-z,stack-size=2097152" \
    --toolchain=hardened \
    --disable-autodetect \
    --disable-debug \
    --disable-doc \
    --disable-ffplay \
    --disable-network \
    --disable-shared \
    --enable-static \
    --enable-gpl \
    --enable-version3 \
    --enable-bzlib \
    --enable-zlib \
    --enable-libaom \
    --enable-libdav1d \
    --enable-libmp3lame \
    --enable-libopus \
    --enable-libvorbis \
    --enable-libvpx \
    --enable-libx264 \
    --enable-libx265 \
  || (cat ffbuild/config.log ; false) \
  && make -j$(nproc) install

# Fail the build unless both binaries are static, relro, bind-now, PIE, with a non-executable stack.
RUN \
  for f in /usr/local/bin/ffmpeg /usr/local/bin/ffprobe; do \
    file "$f" && \
    test "$(ldd "$f" | wc -l)" -eq 1 && \
    readelf -l "$f" | grep -q GNU_RELRO && \
    readelf -d "$f" | grep -q BIND_NOW && \
    readelf -h "$f" | grep -q DYN && \
    readelf -W -l "$f" | grep GNU_STACK | grep -q -v RWE || exit 1; \
  done

FROM scratch AS final
COPY --from=builder /usr/local/bin/ffmpeg /usr/local/bin/ffprobe /

# Smoke test each encoder.
RUN ["/ffmpeg", "-hide_banner", "-version"]
RUN ["/ffprobe", "-hide_banner", "-version"]
RUN ["/ffmpeg", "-v", "error", "-f", "lavfi", "-i", "testsrc", "-c:v", "libx264", "-t", "100ms", "-f", "null", "-"]
RUN ["/ffmpeg", "-v", "error", "-f", "lavfi", "-i", "testsrc", "-c:v", "libx265", "-t", "100ms", "-f", "null", "-"]
RUN ["/ffmpeg", "-v", "error", "-f", "lavfi", "-i", "testsrc", "-c:v", "libvpx", "-t", "100ms", "-f", "null", "-"]
RUN ["/ffmpeg", "-v", "error", "-f", "lavfi", "-i", "testsrc", "-c:v", "libvpx-vp9", "-t", "100ms", "-f", "null", "-"]
RUN ["/ffmpeg", "-v", "error", "-f", "lavfi", "-i", "testsrc", "-c:v", "libaom-av1", "-cpu-used", "8", "-t", "100ms", "-f", "null", "-"]
RUN ["/ffmpeg", "-v", "error", "-f", "lavfi", "-i", "sine", "-c:a", "aac", "-t", "100ms", "-f", "null", "-"]
RUN ["/ffmpeg", "-v", "error", "-f", "lavfi", "-i", "sine", "-c:a", "libopus", "-t", "100ms", "-f", "null", "-"]
RUN ["/ffmpeg", "-v", "error", "-f", "lavfi", "-i", "sine", "-c:a", "libvorbis", "-t", "100ms", "-f", "null", "-"]
RUN ["/ffmpeg", "-v", "error", "-f", "lavfi", "-i", "sine", "-c:a", "libmp3lame", "-t", "100ms", "-f", "null", "-"]

# One layer for the two binaries.
FROM scratch
COPY --from=final / /
LABEL maintainer="Vincent Giersch <vincent+docker@tutteo.com>"
ENTRYPOINT ["/ffmpeg"]
