# inspired by https://github.com/hsp-panda/dockerfiles-panda/tree/main/ros_realsense
FROM ros:noetic

LABEL maintainer="jean-baptiste.mouret@inria.fr"

ENV ROS_DISTRO noetic

ENV DEBIAN_FRONTEND="noninteractive" TZ="Europe/Paris"

RUN apt -q -qq update && \
  DEBIAN_FRONTEND=noninteractive apt install -y --allow-unauthenticated \
  libsdl2-dev \
  ros-${ROS_DISTRO}-catkin \
  git \
  clang \
  python3-catkin \
  python3-catkin-pkg \
  python3-catkin-tools


RUN . /opt/ros/${ROS_DISTRO}/setup.sh && mkdir -p /catkin_ws/src && cd /catkin_ws/ && catkin build

RUN cd /catkin_ws/src/ && git clone --recursive https://github.com/hucebot/ros_whisper.git

RUN cd /catkin_ws/src/ros_whisper/whisper.cpp \
  && mkdir build \
  && cd build \
  && cmake .. -DWHISPER_SUPPORT_SDL2  \
  && make -j \
  && make install 

# TODO : download models

RUN rosdep update


