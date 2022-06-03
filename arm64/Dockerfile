FROM ubuntu

LABEL maintainer="Jiangda He. mail: hejiangda@gmail.com"

ENV OPENCV_VERSION=4.5.5
ENV BUILD="unzip \
    wget \
    build-essential \
    cmake \
    curl \
    git \
    gcc-arm-linux-gnueabi \
    gcc-arm-linux-gnueabihf \
    libgtk2.0-dev \
    pkg-config \
    libavcodec-dev \
    libavformat-dev \
    libswscale-dev \
    libtbb2 \
    libtbb-dev \
    libjpeg-dev \
    libpng-dev \
    libtiff-dev \
    libdc1394-dev"

ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN apt-get update && apt-get install -y ${BUILD}

RUN mkdir /tmp/opencv && \
    cd /tmp/opencv && \
    wget -O opencv.zip https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.zip && \
    unzip opencv.zip && \
    wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/${OPENCV_VERSION}.zip && \
    unzip opencv_contrib.zip && \
    mkdir /tmp/opencv/opencv-${OPENCV_VERSION}/build

WORKDIR /tmp/opencv/opencv-${OPENCV_VERSION}/build

RUN apt-get install g++-aarch64-linux-gnu gcc-aarch64-linux-gnu -y

RUN cmake \
    -D CMAKE_TOOLCHAIN_FILE=/tmp/opencv/opencv-${OPENCV_VERSION}/platforms/linux/aarch64-gnu.toolchain.cmake \
    -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D OPENCV_EXTRA_MODULES_PATH=/tmp/opencv/opencv_contrib-${OPENCV_VERSION}/modules \
    -D WITH_FFMPEG=OFF \
    -D WITH_GTK=OFF \
    -D WITH_1394=OFF \
    -D WITH_TIFF=OFF \
    -D INSTALL_C_EXAMPLES=NO \
    -D INSTALL_PYTHON_EXAMPLES=NO \
    -D BUILD_ANDROID_EXAMPLES=NO \
    -D BUILD_DOCS=NO \
    -D BUILD_TESTS=NO \
    -D BUILD_PERF_TESTS=NO \
    -D BUILD_EXAMPLES=NO \
    -D BUILD_opencv_java=NO \
    -D BUILD_opencv_python=NO \
    -D BUILD_opencv_python2=NO \
    -D BUILD_opencv_python3=NO \
    -D BUILD_SHARED_LIBS=NO \
    -D OPENCV_GENERATE_PKGCONFIG=YES .. && \
    make -j12 && \
    make install && \
    cd && rm -rf /tmp/opencv

ENV CGO_CPPFLAGS -I/usr/local/include/opencv4
ENV PKG_CONFIG_PATH /usr/local/lib/pkgconfig
ENV LD_LIBRARY_PATH /usr/local/lib
ENV CGO_CXXFLAGS "--std=c++1z"
#ENV CGO_LDFLAGS "--static -L/usr/local/lib -lopencv_core -lopencv_face -lopencv_videoio -lopencv_imgproc -lopencv_highgui -lopencv_imgcodecs -lopencv_objdetect -lopencv_features2d -lopencv_video -lopencv_dnn -lopencv_xfeatures2d -lopencv_plot -lopencv_tracking"
ENV CGO_LDFLAGS "--static -L/usr/local/lib -L/usr/local/lib/opencv4/3rdparty -lopencv_gapi -lopencv_stitching -lopencv_aruco -lopencv_bgsegm -lopencv_bioinspired -lopencv_ccalib -lopencv_dnn_objdetect -lopencv_dnn_superres -lopencv_dpm -lopencv_highgui -lopencv_face  -lopencv_fuzzy -lopencv_hfs -lopencv_img_hash -lopencv_intensity_transform -lopencv_line_descriptor -lopencv_mcc -lopencv_quality -lopencv_rapid -lopencv_reg -lopencv_rgbd -lopencv_saliency -lopencv_stereo -lopencv_structured_light -lopencv_phase_unwrapping -lopencv_superres -lopencv_optflow -lopencv_surface_matching -lopencv_tracking -lopencv_datasets -lopencv_text -lopencv_dnn -lopencv_plot -lopencv_videostab -lopencv_videoio -lopencv_xfeatures2d -lopencv_shape -lopencv_ml -lopencv_ximgproc -lopencv_video -lopencv_xobjdetect -lopencv_objdetect -lopencv_calib3d -lopencv_imgcodecs -lopencv_features2d -lopencv_flann -lopencv_xphoto -lopencv_photo -lopencv_imgproc -lopencv_core -littnotify -llibprotobuf -llibwebp -llibopenjp2 -lquirc   -lade  -lm -lpthread -lrt -lc -ldl"
COPY version /version

WORKDIR /tmp
RUN wget https://go.dev/dl/go1.18.linux-amd64.tar.gz
RUN rm -rf /usr/local/go && tar -C /usr/local -xzf go1.18.linux-amd64.tar.gz
ENV PATH $PATH:/usr/local/go/bin
RUN go version
WORKDIR /version
RUN go mod init gocv-version && go mod tidy &&  CGO_ENABLED=1 GOOS=linux GOARCH=arm64 CC=/usr/bin/aarch64-linux-gnu-gcc go build -tags static -o . && ./gocv-version

