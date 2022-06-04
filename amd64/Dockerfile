FROM ubuntu

LABEL maintainer="Jiangda He. mail: hejiangda@gmail.com"

ENV OPENCV_VERSION=4.5.5

ENV BUILD="unzip \
    wget \
    build-essential \
    cmake \
    curl \
    git \
    libz-dev\
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
    -D WITH_IPP=OFF \
    -D BUILD_SHARED_LIBS=NO \
    -D OPENCV_GENERATE_PKGCONFIG=YES \
    .. && \
    cd /tmp/opencv/opencv-${OPENCV_VERSION}/build && make -j12 && \
    make install -j12 && \
    rm -rf /tmp/opencv/

RUN wget https://go.dev/dl/go1.18.linux-amd64.tar.gz && rm -rf /usr/local/go && tar -C /usr/local -xzf go1.18.linux-amd64.tar.gz && rm -rf go1.18.linux-amd64.tar.gz
ENV PATH=$PATH:/usr/local/go/bin
ENV CGO_CPPFLAGS="-I/usr/local/include/opencv4"
ENV CGO_LDFLAGS="--static -L/usr/local/lib  -L/usr/local/lib/opencv4/3rdparty -lopencv_world -lopencv_img_hash -lade -llibopenjp2 -llibwebp -littnotify -llibpng -lquirc -llibjpeg-turbo -llibprotobuf -ltbb -lIlmImf -ldl -lpthread -lm -lz"
WORKDIR /root/
#COPY dictpenuiautomationtest-master.zip /root/
#RUN unzip dictpenuiautomationtest-master.zip
#RUN mkdir -p /root/dictpenuiautomationtest-master/globalVariable/dictpenUiAutomaticTest
#RUN touch /root/dictpenuiautomationtest-master/globalVariable/dictpenUiAutomaticTest/clickPhysicsButtonExecutableArm32
#RUN touch /root/dictpenuiautomationtest-master/globalVariable/dictpenUiAutomaticTest/sendEventArm32
#RUN touch /root/dictpenuiautomationtest-master/globalVariable/dictpenUiAutomaticTest/slipExecutableArm32
#RUN cd /root/dictpenuiautomationtest-master &&  go build -tags "customenv embed"
