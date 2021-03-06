###############################################################################
# ROS 2 Dashing for development
###############################################################################
FROM ubuntu:bionic

RUN apt update \
&&  apt install --no-install-recommends -y \
        ca-certificates \
        curl \
        gnupg2 \
        lsb-release \
        locales \
&&  apt clean

RUN locale-gen en_US en_US.UTF-8 && update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
ENV LANG en_US.UTF-8

RUN curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | apt-key add - \
&&  sh -c 'echo "deb [arch=amd64,arm64] http://packages.ros.org/ros2/ubuntu `lsb_release -cs` main" > /etc/apt/sources.list.d/ros2-latest.list' \

ENV TZ Europe/Madrid
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt update \
&&  apt install --no-install-recommends -y \
        build-essential \
        gdb \
        cmake \
        git \
        python3-colcon-common-extensions \
        python3-pip \
        python-rosdep \
        python3-vcstool \
        wget \
&&  apt clean

RUN python3 -m pip install -U \
        argcomplete \
        flake8 \
        flake8-blind-except \
        flake8-builtins \
        flake8-class-newline \
        flake8-comprehensions \
        flake8-deprecated \
        flake8-docstrings \
        flake8-import-order \
        flake8-quotes \
        pytest-repeat \
        pytest-rerunfailures \
        pytest \
        pytest-cov \
        pytest-runner \
        setuptools

RUN apt update \
&&  apt install --no-install-recommends -y \
    libasio-dev \
    libtinyxml2-dev \
&&  apt clean

RUN apt update \
&&  apt install --no-install-recommends -y \
    libcunit1-dev \
&&  apt clean

RUN mkdir -p ~/ros2_eloquent/src
RUN cd ~/ros2_eloquent \
&&  wget https://raw.githubusercontent.com/ros2/ros2/eloquent-release/ros2.repos \
&&  vcs import src < ros2.repos \
&&  rosdep init \
&&  rosdep update \
&&  rosdep install --from-paths src --ignore-src --rosdistro eloquent -y --skip-keys "console_bridge fastcdr fastrtps libopensplice67 libopensplice69 rti-connext-dds-5.3.1 urdfdom_headers" \
&&  colcon build --cmake-args -DCMAKE_BUILD_TYPE=Debug \
&&  rm -rf ./build ./log