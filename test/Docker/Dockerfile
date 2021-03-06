FROM debian:8
MAINTAINER Insight Software Consortium <community@itk.org>

# Insert this line before "RUN apt-get update" to dynamically
# replace httpredir.debian.org with a single working domain
# in attempt to "prevent" the "Error reading from server" error.
RUN apt-get update && apt-get install -y curl && \
  sed -i "s/httpredir.debian.org/`curl -s -D - http://httpredir.debian.org/demo/debian/ | awk '/^Link:/ { print $2 }' | sed -e 's@<http://\(.*\)/debian/>;@\1@g'`/" /etc/apt/sources.list

RUN apt-get update && apt-get install -y \
  build-essential \
  curl \
  cmake \
  git \
  libexpat1-dev \
  libhdf5-dev \
  libjpeg-dev \
  libpng12-dev \
  libtiff5-dev \
  ninja-build \
  python \
  wget \
  vim \
  zlib1g-dev

WORKDIR /usr/src

# ITK release 2016-09-11
ENV ITK_GIT_TAG 049183e82919275d4f4c3273bbf80738ee469527
RUN git clone https://itk.org/ITK.git && \
  cd ITK && \
  git checkout ${ITK_GIT_TAG} && \
  cd ../ && \
  mkdir ITK-build && \
  cd ITK-build && \
  cmake \
    -G Ninja \
    -DCMAKE_INSTALL_PREFIX:PATH=/usr \
    -DBUILD_EXAMPLES:BOOL=OFF \
    -DBUILD_TESTING:BOOL=OFF \
    -DBUILD_SHARED_LIBS:BOOL=ON \
    -DCMAKE_POSITION_INDEPENDENT_CODE:BOOL=ON \
    -DITK_LEGACY_REMOVE:BOOL=ON \
    -DITK_BUILD_DEFAULT_MODULES:BOOL=OFF \
    -DITK_USE_SYSTEM_LIBRARIES:BOOL=ON \
    -DModule_ITKIOImageBase:BOOL=ON \
    -DModule_ITKIOSpatialObjects:BOOL=ON \
    -DModule_ITKMetaIO:BOOL=ON \
    -DModule_ITKImageGradient:BOOL=ON \
    -DModule_ITKTestKernel:BOOL=ON \
    ../ITK && \
  ninja && \
  find . -name '*.o' -delete

RUN mkdir -p /usr/src/ITKAnisotropicDiffusionLBR-build
