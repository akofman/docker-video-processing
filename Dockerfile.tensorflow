FROM  tensorflow/tensorflow:1.4.1-py3
LABEL maintainer="Alexis Kofman <alexis.kofman@gmail.com>"

ENV PKG_CONFIG_PATH="/usr/local/lib/pkgconfig" \
    LD_LIBRARY_PATH="/usr/local/lib" \
    SRC="/usr/local" \
    OPENCV_VERSION="3.3.1" \
    FFMPEG_VERSION="3.4" \
    FDKAAC_VERSION="0.1.5" \
    LAME_VERSION="3.99.5" \
    OGG_VERSION="1.3.3" \
    OPENCOREAMR_VERSION="0.1.5" \
    OPUS_VERSION="1.2.1" \
    THEORA_VERSION="1.1.1" \
    VORBIS_VERSION="1.3.5" \
    VPX_VERSION="1.6.1" \
    X264_VERSION="20171204-2245-stable" \
    X265_VERSION="2.6" \
    XVID_VERSION="1.3.4" \
    FREETYPE_VERSION="2.8.1" \
    LIBVIDSTAB_VERSION="1.1.0" \
    OPENCV_SHA256SUM="5dca3bb0d661af311e25a72b04a7e4c22c47c1aa86eb73e70063cd378a2aa6ee  3.3.1.tar.gz" \
    FFMPEG_SHA256SUM="6ed03b00404a3923e3c2f560248a9c9ad79fbaaee26d723f74aae6b31fe2bae6  ffmpeg-3.4.tar.gz" \
    FDKAAC_SHA256SUM="ff53d1d01cacc29c071e23192dfefa93bdbeaf775fc5d296259b4859d0306b79  v0.1.5.tar.gz" \
    LAME_SHA256SUM="24346b4158e4af3bd9f2e194bb23eb473c75fb7377011523353196b19b9a23ff  lame-3.99.5.tar.gz" \
    OGG_SHA256SUM="c2e8a485110b97550f453226ec644ebac6cb29d1caef2902c007edab4308d985  libogg-1.3.3.tar.gz" \
    OPENCOREAMR_SHA256SUM="2c006cb9d5f651bfb5e60156dbff6af3c9d35c7bbcc9015308c0aff1e14cd341  opencore-amr-0.1.5.tar.gz" \
    OPUS_SHA256SUM="cfafd339ccd9c5ef8d6ab15d7e1a412c054bf4cb4ecbbbcc78c12ef2def70732  opus-1.2.1.tar.gz" \
    THEORA_SHA256SUM="40952956c47811928d1e7922cda3bc1f427eb75680c3c37249c91e949054916b  libtheora-1.1.1.tar.gz" \
    VORBIS_SHA256SUM="6efbcecdd3e5dfbf090341b485da9d176eb250d893e3eb378c428a2db38301ce  libvorbis-1.3.5.tar.gz" \
    VPX_SHA256SUM="cda8bb6f0e4848c018177d3a576fa83ed96d762554d7010fe4cfb9d70c22e588  v1.6.1" \
    X264_SHA256SUM="73eeeed30b5c5dd2ac677383f123cf03ed05a38ef12cfcf78559fe468fdec46f  x264-snapshot-20171204-2245-stable.tar.bz2" \
    X265_SHA256SUM="1bf0036415996af841884802161065b9e6be74f5f6808ac04831363e2549cdbf  x265_2.6.tar.gz" \
    XVID_SHA256SUM="4e9fd62728885855bc5007fe1be58df42e5e274497591fec37249e1052ae316f  xvidcore-1.3.4.tar.gz" \
    FREETYPE_SHA256SUM="876711d064a6a1bd74beb18dd37f219af26100f72daaebd2d86cb493d7cd7ec6  freetype-2.8.1.tar.gz" \
    LIBVIDSTAB_SHA256SUM="14d2a053e56edad4f397be0cb3ef8eb1ec3150404ce99a426c4eb641861dc0bb  v1.1.0.tar.gz"

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
                nasm \
                perl \
                pkg-config \
                python \
                libssl-dev \
                yasm \
                zlib1g-dev" && \
    apt-get install -yq --no-install-recommends ${buildDeps} && \
    export MAKEFLAGS="-j$(($(grep -c ^processor /proc/cpuinfo) + 1))" && \
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
    cd x265_v${X265_VERSION}/build/linux && \
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
    curl -sLO https://codeload.github.com/webmproject/libvpx/tar.gz/v${VPX_VERSION} && \
    echo ${VPX_SHA256SUM} | sha256sum --check && \
    tar -zx --strip-components=1 -f v${VPX_VERSION} && \
    ./configure --prefix="${SRC}" --enable-vp8 --enable-vp9 --enable-pic --disable-debug --disable-examples --disable-docs --disable-install-bins --enable-shared && \
    make && \
    make install && \
    rm -rf ${DIR} && \
#RUN  \
## libmp3lame http://lame.sourceforge.net/
    DIR=$(mktemp -d) && cd ${DIR} && \
    curl -sLO https://downloads.sf.net/project/lame/lame/${LAME_VERSION%.*}/lame-${LAME_VERSION}.tar.gz && \
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
    --disable-static \
    --enable-avresample \
    --enable-gpl \
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
    rm -rf ${DIR} && \
# ## INSTALL OPENCV http://opencv.org
#RUN \
    opencvDeps="libopencv-dev qtbase5-dev" && \
    apt-get -yqq install ${opencvDeps} && \
#RUN \
## opencv
    DIR=$(mktemp -d) && cd ${DIR} && \
    # OpenCV depends on NumPy. It represents images as NumPy arrays, so we need to install NumPy
    curl -sLO https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.tar.gz && \
    echo ${OPENCV_SHA256SUM} | sha256sum --check && \    
    tar xzvf ${OPENCV_VERSION}.tar.gz && \
    cd opencv-${OPENCV_VERSION} && \
    mkdir build && cd build && \
    cmake -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D CMAKE_BUILD_TYPE=RELEASE \
    -D FORCE_VTK=ON \
    -D WITH_TBB=ON \
    -D WITH_V4L=ON \
    -D WITH_QT=ON \
    -D WITH_OPENGL=ON \
    -D WITH_CUBLAS=ON \
    -D CUDA_NVCC_FLAGS="-D_FORCE_INLINES --expt-relaxed-constexpr" \
    -D WITH_GDAL=ON \
    -D WITH_XINE=ON \
    -D INSTALL_C_EXAMPLES=OFF \
    -D INSTALL_PYTHON_EXAMPLES=OFF \
    -D BUILD_EXAMPLES=OFF .. && \
    make -j $(($(nproc) + 1)) && \
    make install && \
    /bin/bash -c 'echo "/usr/local/lib" > /etc/ld.so.conf.d/opencv.conf' && \
    ldconfig && \
    rm -rf ${DIR} && \
# ## CLEANUP
    cd && \
    apt-get autoremove -y && \
    apt-get clean -y
