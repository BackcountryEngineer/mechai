FROM osrf/ros:humble-desktop-full

ENV NVIDIA_VISIBLE_DEVICES \
    ${NVIDIA_VISIBLE_DEVICES:-all}
ENV NVIDIA_DRIVER_CAPABILITIES \
    ${NVIDIA_DRIVER_CAPABILITIES:+$NVIDIA_DRIVER_CAPABILITIES,}graphics

RUN curl -sSL http://get.gazebosim.org | sh

RUN apt -y install ros-humble-gazebo-ros-pkgs

RUN useradd --create-home ros && \
        sh -c 'echo "ros ALL=(root) NOPASSWD:ALL" >> /etc/sudoers'

RUN if [ ! -f "/etc/ros/rosdep/sources.list.d/20-default.list" ]; then \
        rosdep init; \
    fi

RUN rosdep update

RUN apt-get install -y python3-apt python3-pip python3-vcstool python3-testresources

RUN pip3 install -U pytest setuptools colcon-ros-bundle
