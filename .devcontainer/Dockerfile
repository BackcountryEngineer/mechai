FROM nvidia/opengl:1.0-glvnd-runtime-ubuntu20.04

ENV DEBIAN_FRONTEND="noninteractive"
ENV QT_X11_NO_MITSHM=1

ENV NVIDIA_VISIBLE_DEVICES \
    ${NVIDIA_VISIBLE_DEVICES:-all}
ENV NVIDIA_DRIVER_CAPABILITIES \
    ${NVIDIA_DRIVER_CAPABILITIES:+$NVIDIA_DRIVER_CAPABILITIES,}graphics

RUN apt-get update && apt-get install -y --no-install-recommends \
        ca-certificates \
        gnupg2 \
        mesa-utils \
        sudo \
        unzip \
        wget \
        curl \
        lsb-release \
        vim

RUN apt update && apt -y install locales && \
        locale-gen en_US en_US.UTF-8 && \
        update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8

ENV LANG=en_US.UTF-8

RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key  -o /usr/share/keyrings/ros-archive-keyring.gpg && \
        curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | apt-key add - && \
        echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" | tee /etc/apt/sources.list.d/ros2.list > /dev/null && \
        apt update && \
        apt install -y ros-galactic-desktop && \
        /bin/bash -c "source /opt/ros/galactic/setup.bash"

RUN apt install -y \
        ros-galactic-gazebo-ros-pkgs \
        ros-galactic-xacro \
        ros-galactic-joint-state-publisher-gui \
        ros-galactic-navigation2
        # ros-galactic-nav2-bringup
        # '~ros-galactic-turtlebot3-.*'

RUN apt-key adv --fetch-keys 'http://packages.osrfoundation.org/gazebo.key' && \
        apt update && \
        apt install -y python3-rosdep git

RUN if [ ! -f "/etc/ros/rosdep/sources.list.d/20-default.list" ]; then \
        rosdep init; \
    fi

RUN rosdep update

RUN apt-get install -y python3-apt python3-pip python3-vcstool python3-testresources

RUN pip3 install -U pytest setuptools colcon-ros-bundle

RUN useradd --create-home ros && \
        sh -c 'echo "ros ALL=(root) NOPASSWD:ALL" >> /etc/sudoers'

COPY ./ros_entrypoint.sh /

ENV ROS_DISTRO galactic

RUN sh -c 'echo "source /ros_entrypoint.sh" >> /home/ros/.bashrc'

ENTRYPOINT ["/ros_entrypoint.sh"]
CMD ["bash"]