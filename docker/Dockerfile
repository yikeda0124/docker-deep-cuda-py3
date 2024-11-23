FROM nvidia/cuda:11.8.0-devel-ubuntu20.04
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

# Copy the helper scripts to the working directory.
COPY ./docker/scripts/. /root/scripts/
RUN chmod -R +x /root/scripts/*

# # Force color prompt in terminal.
RUN sed -i 's/#force_color_prompt=yes/force_color_prompt=yes/' /root/.bashrc

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /root

RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh && \
    sh Miniconda3-latest-Linux-x86_64.sh -b -p /opt/miniconda3 && \
    rm -r Miniconda3-latest-Linux-x86_64.sh

ENV PATH /opt/miniconda3/bin:$PATH

RUN pip install --upgrade pip && \
    conda update -n base -c defaults conda && \
    conda create -n lerobot python=3.10 && \
    conda init && \
    echo "conda activate lerobot" >> ~/.bashrc
    
ENV CONDA_DEFAULT_ENV lerobot && \
    PATH /opt/conda/envs/lerobot/bin:$PATH

WORKDIR /root/work
RUN git clone https://github.com/huggingface/lerobot && \
    cd lerobot && \
    conda run -n lerobot pip install -e .
    
ENTRYPOINT []

CMD ["/bin/bash"]

