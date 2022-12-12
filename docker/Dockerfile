FROM ros:noetic

LABEL maintainer="jean-baptiste.mouret@inria.fr"

ENV ROS_DISTRO noetic

ENV DEBIAN_FRONTEND="noninteractive" TZ="Europe/Paris"

RUN apt -q -qq update && \
  DEBIAN_FRONTEND=noninteractive apt install -y --allow-unauthenticated \
  apt-utils \
  libsdl2-dev \
  ros-${ROS_DISTRO}-catkin \
  python3-catkin python3-catkin-tools \ 
  git \
  clang \
  make \
  wget

# fix for cmake (why?)
RUN echo "export PATH=/usr/bin/:$PATH" >> /opt/ros/${ROS_DISTRO}/setup.sh

RUN . /opt/ros/${ROS_DISTRO}/setup.sh && mkdir -p /catkin_ws/src && cd /catkin_ws/ && catkin build

RUN cd /catkin_ws/src/ && git clone --recursive https://github.com/hucebot/ros_whisper.git

RUN . /opt/ros/${ROS_DISTRO}/setup.sh \
  && cd /catkin_ws/src/ros_whisper/whisper.cpp \
  && mkdir build \
  && cd build \
  && cmake .. -DWHISPER_SUPPORT_SDL2=True  \
  && make -j

RUN rosdep update

RUN cd /catkin_ws/src/ros_whisper/whisper.cpp \
  && bash ./models/download-ggml-model.sh base.en \
  && bash ./models/download-ggml-model.sh base \
  && bash ./models/download-ggml-model.sh small.en \
  && bash ./models/download-ggml-model.sh small \
  && bash ./models/download-ggml-model.sh tiny.en \
  && bash ./models/download-ggml-model.sh tiny

# each medium is 1.5gb
#  && bash ./models/download-ggml-model.sh medium \
#  && bash ./models/download-ggml-model.sh medium.en \
# large would 5Gb more...




