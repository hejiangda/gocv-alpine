FROM ubuntu
# aarch64
LABEL maintainer="Jiangda He. mail: hejiangda@gmail.com"

ENV OPENCV_VERSION=4.5.5

ENV BUILD="unzip \
    wget \
    crossbuild-essential-arm64 \
    cmake \
    curl \
    git \
    ca-certificates \
    pkg-config"

ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN apt-get update && apt-get install -y --no-install-recommends ${BUILD}

RUN mkdir /tmp/opencv && \
    cd /tmp/opencv && \
    wget -O opencv.zip https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.zip && \
    unzip opencv.zip && \
    wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/${OPENCV_VERSION}.zip && \
    unzip opencv_contrib.zip && \
    mkdir /tmp/opencv/opencv-${OPENCV_VERSION}/build && \
    rm opencv.zip opencv_contrib.zip && \
    cd /tmp/opencv/opencv-${OPENCV_VERSION}/build && \
    cmake \
    -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D OPENCV_EXTRA_MODULES_PATH=/tmp/opencv/opencv_contrib-${OPENCV_VERSION}/modules \
    -D WITH_FFMPEG=OFF \
    -D WITH_GTK=OFF \
    -D WITH_1394=OFF \
    -D WITH_TIFF=OFF \
    -D BUILD_opencv_world=ON \
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
    -D WITH_TBB=ON \
    -D BUILD_TBB=ON \
    -D BUILD_SHARED_LIBS=NO \
    -D OPENCV_GENERATE_PKGCONFIG=YES \
    -D CMAKE_TOOLCHAIN_FILE=/tmp/opencv/opencv-${OPENCV_VERSION}/platforms/linux/aarch64-gnu.toolchain.cmake .. && \
    cd /tmp/opencv/opencv-${OPENCV_VERSION}/build && make -j12 && \
    make install -j12 && \
    rm -rf /tmp/opencv/

RUN wget https://go.dev/dl/go1.18.linux-amd64.tar.gz && rm -rf /usr/local/go && tar -C /usr/local -xzf go1.18.linux-amd64.tar.gz && rm -rf go1.18.linux-amd64.tar.gz
ENV PATH=$PATH:/usr/local/go/bin
ENV CGO_CPPFLAGS="-I/usr/local/include/opencv4"
ENV CGO_LDFLAGS="--static  -L/usr/local/lib  -L/usr/local/lib/opencv4/3rdparty -lopencv_world -lopencv_img_hash -lade -llibopenjp2 -llibwebp -ltegra_hal -littnotify -llibpng -lquirc -lzlib -llibjpeg-turbo -llibprotobuf -ltbb -ldl -lpthread -lm"
ENV CGO_ENABLED=1
ENV GOOS=linux
ENV GOARCH=arm64
ENV CC=aarch64-linux-gnu-gcc
ENV CXX=aarch64-linux-gnu-g++
WORKDIR /root/
#COPY dictpenuiautomationtest-master.zip /root/
#RUN unzip dictpenuiautomationtest-master.zip
#RUN mkdir -p /root/dictpenuiautomationtest-master/globalVariable/dictpenUiAutomaticTest
#RUN touch /root/dictpenuiautomationtest-master/globalVariable/dictpenUiAutomaticTest/clickPhysicsButtonExecutableArm32
#RUN touch /root/dictpenuiautomationtest-master/globalVariable/dictpenUiAutomaticTest/sendEventArm32
#RUN touch /root/dictpenuiautomationtest-master/globalVariable/dictpenUiAutomaticTest/slipExecutableArm32
#RUN cd /root/dictpenuiautomationtest-master && CGO_ENABLED=1 GOOS=linux GOARCH=arm64 CC=aarch64-linux-gnu-gcc CXX=aarch64-linux-gnu-g++  go build -tags "customenv embed"
