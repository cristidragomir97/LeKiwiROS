# ─── Builder Stage ────────────────────────────────────────────────────────────
FROM docker.io/dragomirxyz/robocore_humble_arm64 AS builder

ENV DEBIAN_FRONTEND=noninteractive
ENV ROS_DISTRO=humble
ENV ROS_WS=/ros_ws

# Install core build tools
RUN apt-get update && apt-get install -y \
    git \
    python3-colcon-common-extensions \
    sudo \
    curl \
    gnupg2 \
    lsb-release \
    udev \
    cmake \
    python3-rosdep \
    python3-vcstool \
    build-essential \
  && rm -rf /var/lib/apt/lists/*

# Copy in source tree
WORKDIR /ros_ws
COPY ros_ws/src ./src


# Install ROS dependencies
RUN bash -lc "\
    apt-get update && \
    source /opt/ros/${ROS_DISTRO}/setup.bash && \
    rosdep update && \
    rosdep install --from-paths src --ignore-src -y && \
    rm -rf /var/lib/apt/lists/* \
"

# Build the workspace
RUN bash -lc "\
    source /opt/ros/${ROS_DISTRO}/setup.bash && \
    colcon build --symlink-install --install-base install \
"