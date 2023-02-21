FROM nvidia/cuda:11.5.1-devel-ubuntu20.04
SHELL ["/bin/bash", "-c"]

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y \
        wget \
        curl \
        git \
        vim \
        unzip \
        python3-pip \
        lsb-release \
        gnupg \
        avahi-autoipd \
        avahi-daemon \
        avahi-utils \
        libgl1-mesa-dev \
        libglib2.0-0 \
        libxrandr2 \
        libxcursor1 \
        libglfw3 \
        libglew2.1 \
        libxinerama1


RUN source /root/.bashrc && \
    pip install matplotlib && \
    pip install torch==1.11.0+cu115 torchvision==0.12.0+cu115 torchaudio==0.11.0 --extra-index-url https://download.pytorch.org/whl/cu115

# Copy the helper scripts to the working directory.
COPY ./docker/scripts/. /root/scripts/
RUN chmod -R +x /root/scripts/*

# # Force color prompt in terminal.
RUN sed -i 's/#force_color_prompt=yes/force_color_prompt=yes/' /root/.bashrc

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /root


# mujocoのインストール
RUN mkdir /root/.mujoco && \
    cd /root/.mujoco && \
    wget https://www.roboti.us/download/mujoco200_linux.zip && \
    unzip mujoco200_linux.zip && \
    wget https://www.roboti.us/file/mjkey.txt && \
    cp mjkey.txt mujoco200_linux/bin && \
    cp -r mujoco200_linux mujoco200

RUN echo "export LD_LIBRARY_PATH=/root/.mujoco/mujoco200/bin${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}" >> /root/.bashrc
RUN echo "export MUJOCO_KEY_PATH=/root/.mujoco${MUJOCO_KEY_PATH}" >> /root/.bashrc


ENTRYPOINT []

CMD ["/bin/bash"]

