FROM  node:8.14.0-jessie
LABEL maintainer="Alexis Kofman <alexis.kofman@gmail.com>"

ENV PKG_CONFIG_PATH="/usr/local/lib/pkgconfig" \
    LD_LIBRARY_PATH="/usr/local/lib" \
    SRC="/usr/local" \
    FFMPEG_VERSION="4.1" \
    FDKAAC_VERSION="0.1.6" \
    FRIBIDI_VERSION="0.19.7" \
    LAME_VERSION="3.100" \
    LIBASS_VERSION="0.14.0" \
    OGG_VERSION="1.3.3" \
    OPENCOREAMR_VERSION="0.1.5" \
    OPUS_VERSION="1.2.1" \
    THEORA_VERSION="1.1.1" \
    VORBIS_VERSION="1.3.6" \
    VPX_VERSION="1.7.0" \
    X264_VERSION="20181015-2245-stable" \
    X265_VERSION="2.9" \
    XVID_VERSION="1.3.5" \
    FREETYPE_VERSION="2.9.1" \
    LIBVIDSTAB_VERSION="1.1.0" \
    FFMPEG_SHA256SUM="e7196ba28d7527daee917babdcbebb56b04c77e0b20593f2dd0bc11528a27066  ffmpeg-4.1.tar.gz" \
    FDKAAC_SHA256SUM="adbcd793e406e1b88b3c1c41382d49f8c27371485b823c0fdab69c9124fd2ce3  v0.1.6.tar.gz" \
    FREETYPE_SHA256SUM="ec391504e55498adceb30baceebd147a6e963f636eb617424bcfc47a169898ce  freetype-2.9.1.tar.gz" \
    FRIBIDI_SHA256SUM="3fc96fa9473bd31dcb5500bdf1aa78b337ba13eb8c301e7c28923fea982453a8  0.19.7.tar.gz" \
    LAME_SHA256SUM="ddfe36cab873794038ae2c1210557ad34857a4b6bdc515785d1da9e175b1da1e  lame-3.100.tar.gz" \
    LIBASS_SHA256SUM="82e70ee1f9afe2e54ab4bf6510b83bb563fcb2af978f0f9da82e2dbc9ae0fd72  0.14.0.tar.gz" \
    LIBVIDSTAB_SHA256SUM="14d2a053e56edad4f397be0cb3ef8eb1ec3150404ce99a426c4eb641861dc0bb  v1.1.0.tar.gz" \
    OGG_SHA256SUM="c2e8a485110b97550f453226ec644ebac6cb29d1caef2902c007edab4308d985  libogg-1.3.3.tar.gz" \
    OPENCOREAMR_SHA256SUM="2c006cb9d5f651bfb5e60156dbff6af3c9d35c7bbcc9015308c0aff1e14cd341  opencore-amr-0.1.5.tar.gz" \
    OPUS_SHA256SUM="cfafd339ccd9c5ef8d6ab15d7e1a412c054bf4cb4ecbbbcc78c12ef2def70732  opus-1.2.1.tar.gz" \
    THEORA_SHA256SUM="40952956c47811928d1e7922cda3bc1f427eb75680c3c37249c91e949054916b  libtheora-1.1.1.tar.gz" \
    VORBIS_SHA256SUM="6ed40e0241089a42c48604dc00e362beee00036af2d8b3f46338031c9e0351cb  libvorbis-1.3.6.tar.gz" \
    VPX_SHA256SUM="1fec931eb5c94279ad219a5b6e0202358e94a93a90cfb1603578c326abfc1238  v1.7.0.tar.gz" \
    X264_SHA256SUM="e1510f9cc9c70c8f7c83cec3b079d035e92de5b4c76bf3185234c196c6b82622  x264-snapshot-20181015-2245-stable.tar.bz2" \
    X265_SHA256SUM="ebae687c84a39f54b995417c52a2fdde65a4e2e7ebac5730d251471304b91024  x265_2.9.tar.gz" \
    XVID_SHA256SUM="165ba6a2a447a8375f7b06db5a3c91810181f2898166e7c8137401d7fc894cf0  xvidcore-1.3.5.tar.gz"

## INSTALL FFMPEG https://ffmpeg.org
RUN apt-get -yqq update && \
    apt-get install -yq --no-install-recommends ca-certificates expat libgomp1 && \
    buildDeps="autoconf \
                automake \
                cmake \
                curl \
                bzip2 \
                libexpat1-dev \
                g++ \
                gcc \
                git \
                gperf \
                libtool \
                make \
                perl \
                pkg-config \
                python \
                libssl-dev \
                yasm \
                zlib1g-dev" && \
    apt-get -yqq update && \
    apt-get install -yq --no-install-recommends ${buildDeps} && \
    apt-get autoremove -y && \
    apt-get clean -y && \
    export MAKEFLAGS="-j$(($(grep -c ^processor /proc/cpuinfo) + 1))" && \
#RUN \
## nasm 2.13
    DIR=$(mktemp -d) && cd ${DIR} && \
    curl -sLO http://www.nasm.us/pub/nasm/releasebuilds/2.13.01/nasm-2.13.01.tar.gz && \
    tar -zx -f nasm-2.13.01.tar.gz && \
    cd nasm-2.13.01 && \
    ./configure && \
    make && \
    make install && \
    rm -rf ${DIR} && \
#RUN  \
## opencore-amr https://sourceforge.net/projects/opencore-amr/
    DIR=$(mktemp -d) && cd ${DIR} && \
    curl -sLO https://downloads.sf.net/project/opencore-amr/opencore-amr/opencore-amr-${OPENCOREAMR_VERSION}.tar.gz && \
    echo ${OPENCOREAMR_SHA256SUM} | sha256sum --check && \
    tar -zx --strip-components=1 -f opencore-amr-${OPENCOREAMR_VERSION}.tar.gz && \
    ./configure --prefix="${SRC}" --bindir="${SRC}/bin" --enable-shared --datadir=${DIR} && \
    make && \
    make install && \
    rm -rf ${DIR} && \
#RUN  \
## x264 http://www.videolan.org/developers/x264.html
    DIR=$(mktemp -d) && cd ${DIR} && \
    curl -sLO ftp://ftp.videolan.org/pub/videolan/x264/snapshots/x264-snapshot-${X264_VERSION}.tar.bz2 && \
    echo ${X264_SHA256SUM} | sha256sum --check && \
    tar -jx --strip-components=1 -f x264-snapshot-${X264_VERSION}.tar.bz2 && \
    ./configure --prefix="${SRC}" --bindir="${SRC}/bin" --enable-pic --enable-shared --disable-cli && \
    make && \
    make install && \
    rm -rf ${DIR} && \
#RUN  \
## x265 http://x265.org/
    DIR=$(mktemp -d) && cd ${DIR} && \
    curl -sLO https://download.videolan.org/pub/videolan/x265/x265_${X265_VERSION}.tar.gz  && \
    echo ${X265_SHA256SUM} | sha256sum --check && \
    tar -zx -f x265_${X265_VERSION}.tar.gz && \
    cd x265_${X265_VERSION}/build/linux && \
    ./multilib.sh && \
    make -C 8bit install && \
    rm -rf ${DIR} && \
#RUN  \
## libogg https://www.xiph.org/ogg/
    DIR=$(mktemp -d) && cd ${DIR} && \
    curl -sLO http://downloads.xiph.org/releases/ogg/libogg-${OGG_VERSION}.tar.gz && \
    echo ${OGG_SHA256SUM} | sha256sum --check && \
    tar -zx --strip-components=1 -f libogg-${OGG_VERSION}.tar.gz && \
    ./configure --prefix="${SRC}" --bindir="${SRC}/bin" --disable-static --datarootdir=${DIR} && \
    make && \
    make install && \
    rm -rf ${DIR} && \
#RUN  \
## libopus https://www.opus-codec.org/
    DIR=$(mktemp -d) && cd ${DIR} && \
    curl -sLO https://archive.mozilla.org/pub/opus/opus-${OPUS_VERSION}.tar.gz && \
    echo ${OPUS_SHA256SUM} | sha256sum --check && \
    tar -zx --strip-components=1 -f opus-${OPUS_VERSION}.tar.gz && \
    autoreconf -fiv && \
    ./configure --prefix="${SRC}" --disable-static --datadir="${DIR}" && \
    make && \
    make install && \
    rm -rf ${DIR} && \
#RUN  \
## libvorbis https://xiph.org/vorbis/
    DIR=$(mktemp -d) && cd ${DIR} && \
    curl -sLO http://downloads.xiph.org/releases/vorbis/libvorbis-${VORBIS_VERSION}.tar.gz && \
    echo ${VORBIS_SHA256SUM} | sha256sum --check && \
    tar -zx --strip-components=1 -f libvorbis-${VORBIS_VERSION}.tar.gz && \
    ./configure --prefix="${SRC}" --with-ogg="${SRC}" --bindir="${SRC}/bin" \
    --disable-static --datadir="${DIR}" && \
    make && \
    make install && \
    rm -rf ${DIR} && \
#RUN  \
## libtheora http://www.theora.org/
    DIR=$(mktemp -d) && cd ${DIR} && \
    curl -sLO http://downloads.xiph.org/releases/theora/libtheora-${THEORA_VERSION}.tar.gz && \
    echo ${THEORA_SHA256SUM} | sha256sum --check && \
    tar -zx --strip-components=1 -f libtheora-${THEORA_VERSION}.tar.gz && \
    ./configure --prefix="${SRC}" --with-ogg="${SRC}" --bindir="${SRC}/bin" \
    --disable-static --datadir="${DIR}" && \
    make && \
    make install && \
    rm -rf ${DIR} && \
#RUN  \
## libvpx https://www.webmproject.org/code/
    DIR=$(mktemp -d) && cd ${DIR} && \
    curl -sLO https://github.com/webmproject/libvpx/archive/v${VPX_VERSION}.tar.gz && \
    echo ${VPX_SHA256SUM} | sha256sum --check && \
    tar -zx --strip-components=1 -f v${VPX_VERSION}.tar.gz && \
    ./configure --prefix="${SRC}" --enable-vp8 --enable-vp9 --enable-pic --disable-debug --disable-examples --disable-docs --disable-install-bins --enable-shared && \
    make && \
    make install && \
    rm -rf ${DIR} && \
#RUN  \
## libmp3lame http://lame.sourceforge.net/
    DIR=$(mktemp -d) && cd ${DIR} && \
    curl -sLO https://downloads.sf.net/project/lame/lame/${LAME_VERSION}/lame-${LAME_VERSION}.tar.gz && \
    echo ${LAME_SHA256SUM} | sha256sum --check && \
    tar -zx --strip-components=1 -f lame-${LAME_VERSION}.tar.gz && \
    ./configure --prefix="${SRC}" --bindir="${SRC}/bin" --disable-static --enable-nasm --datarootdir="${DIR}" && \
    make && \
    make install && \
    rm -rf ${DIR} && \
#RUN  \
## xvid https://www.xvid.com/
    DIR=$(mktemp -d) && cd ${DIR} && \
    curl -sLO http://downloads.xvid.org/downloads/xvidcore-${XVID_VERSION}.tar.gz && \
    echo ${XVID_SHA256SUM} | sha256sum --check && \
    tar -zx -f xvidcore-${XVID_VERSION}.tar.gz && \
    cd xvidcore/build/generic && \
    ./configure --prefix="${SRC}" --bindir="${SRC}/bin" --datadir="${DIR}" --disable-static --enable-shared && \
    make && \
    make install && \
    rm -rf ${DIR} && \
#RUN  \
## fdk-aac https://github.com/mstorsjo/fdk-aac
    DIR=$(mktemp -d) && cd ${DIR} && \
    curl -sLO https://github.com/mstorsjo/fdk-aac/archive/v${FDKAAC_VERSION}.tar.gz && \
    echo ${FDKAAC_SHA256SUM} | sha256sum --check && \
    tar -zx --strip-components=1 -f v${FDKAAC_VERSION}.tar.gz && \
    autoreconf -fiv && \
    ./configure --prefix="${SRC}" --disable-static --datadir="${DIR}" && \
    make && \
    make install && \
    make distclean && \
    rm -rf ${DIR} && \
#RUN  \
## freetype https://www.freetype.org/
    DIR=$(mktemp -d) && cd ${DIR} && \
    curl -sLO http://download.savannah.gnu.org/releases/freetype/freetype-${FREETYPE_VERSION}.tar.gz && \
    echo ${FREETYPE_SHA256SUM} | sha256sum --check && \
    tar -zx --strip-components=1 -f freetype-${FREETYPE_VERSION}.tar.gz && \
    ./configure --disable-static --enable-shared && \
    make && \
    make install && \
    make distclean && \
    rm -rf ${DIR} && \
#RUN  \
## libvstab https://github.com/georgmartius/vid.stab
    DIR=$(mktemp -d) && cd ${DIR} && \
    curl -sLO https://github.com/georgmartius/vid.stab/archive/v${LIBVIDSTAB_VERSION}.tar.gz &&\
    echo ${LIBVIDSTAB_SHA256SUM} | sha256sum --check && \
    tar -zx --strip-components=1 -f v${LIBVIDSTAB_VERSION}.tar.gz && \
    cmake . && \
    make && \
    make install && \
    rm -rf ${DIR} && \
#RUN  \
## fridibi https://www.fribidi.org/
    DIR=$(mktemp -d) && cd ${DIR} && \
    curl -sLO https://github.com/fribidi/fribidi/archive/${FRIBIDI_VERSION}.tar.gz && \
    echo ${FRIBIDI_SHA256SUM} | sha256sum --check && \
    tar -zx --strip-components=1 -f ${FRIBIDI_VERSION}.tar.gz && \
    sed -i 's/^SUBDIRS =.*/SUBDIRS=gen.tab charset lib/' Makefile.am && \
    ./bootstrap --no-config && \
    ./configure -prefix="${SRC}" --disable-static --enable-shared && \
    make -j 1 && \
    make install && \
    rm -rf ${DIR} && \
#RUN  \
## libass https://github.com/libass/libass
    DIR=$(mktemp -d) && cd ${DIR} && \
    curl -sLO https://github.com/libass/libass/archive/${LIBASS_VERSION}.tar.gz &&\
    echo ${LIBASS_SHA256SUM} | sha256sum --check && \
    tar -zx --strip-components=1 -f ${LIBASS_VERSION}.tar.gz && \
    ./autogen.sh && \
    ./configure -prefix="${SRC}" --disable-static --enable-shared && \
    make && \
    make install && \
    rm -rf ${DIR} && \
# RUN  \
## ffmpeg
    DIR=$(mktemp -d) && cd ${DIR} && \
    curl -sLO https://ffmpeg.org/releases/ffmpeg-${FFMPEG_VERSION}.tar.gz && \
    echo ${FFMPEG_SHA256SUM} | sha256sum --check && \
    tar -zx --strip-components=1 -f ffmpeg-${FFMPEG_VERSION}.tar.gz && \
    ./configure \
    --bindir="${SRC}/bin" \
    --disable-debug \
    --disable-doc \
    --disable-ffplay \
    --enable-shared \
    --enable-avresample \
    --enable-gpl \
    --enable-libass \
    --enable-libopencore-amrnb \
    --enable-libopencore-amrwb \
    --enable-libfdk_aac \
    --enable-libfreetype \
    --enable-libvidstab \
    --enable-libmp3lame \
    --enable-libopus \
    --enable-libtheora \
    --enable-libvorbis \
    --enable-libvpx \
    --enable-libx264 \
    --enable-libx265 \
    --enable-libxvid \
    --enable-nonfree \
    --enable-openssl \
    --enable-postproc \
    --enable-shared \
    --enable-small \
    --enable-version3 \
    --extra-cflags="-I${SRC}/include" \
    --extra-ldflags="-L${SRC}/lib" \
    --extra-libs=-ldl \
    --prefix="${SRC}" && \
    make && \
    make install && \
    make distclean && \
    hash -r && \
    cd tools && \
    make qt-faststart && \
    cp qt-faststart ${SRC}/bin && \
    rm -rf ${DIR}
