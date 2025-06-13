FROM nvidia/cuda:12.8.0-cudnn-runtime-ubuntu24.04 as python3.12-base

ENV DEBIAN_FRONTEND noninteractive

RUN uname -a

RUN apt update
RUN apt install -y git curl wget
RUN apt install -y python3-dev python3-tk python3-html5lib python3-apt python3-pip python3-venv python3-setuptools
RUN apt install -y python-is-python3
RUN apt install -y pip

FROM python3.10-base as automatic1111vo

#Install TMalloc
RUN apt install -y libgoogle-perftools-dev

#Install required tools for automatic 1111
RUN apt install -y libgl1
RUN apt install -y libglib2.0-0 libsm6 libxrender1 libxext6
RUN apt install -y vim

#Create new user
RUN useradd -ms /bin/bash appuser

WORKDIR /home/appuser
USER appuser
ENV HOME /home/appuser
ENV PATH="/home/appuser/.local/bin:$PATH"

#Install automatic1111
RUN git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui

VOLUME /home/appuser/stable-diffusion-webui
WORKDIR /home/appuser/stable-diffusion-webui

# Models
WORKDIR /home/appuser/stable-diffusion-webui/models/Stable-diffusion

# epiCPhotoGasm - Last Unicorn
RUN wget --content-disposition 'https://huggingface.co/reszkapl/reszkaplmodels/resolve/main/epicphotogasm_lastUnicorn.safetensors?download=true'

# Realistic Vision V6.0 B1 - V6.0 B1 (VAE)
RUN wget --content-disposition 'https://huggingface.co/reszkapl/reszkaplmodels/resolve/main/realisticVisionV60B1_v60B1VAE.safetensors?download=true'

# RunDiffusionXL - beta
RUN wget --content-disposition 'https://huggingface.co/reszkapl/reszkaplmodels/resolve/main/rundiffusionXL_beta.safetensors?download=true'

# Juggernaut XL - V7 + RunDiffusion
RUN wget --content-disposition 'https://huggingface.co/reszkapl/reszkaplmodels/resolve/main/juggernautXL_v7Rundiffusion.safetensors?download=true'

# RealLife - v3.0
RUN wget --content-disposition 'https://huggingface.co/reszkapl/reszkaplmodels/resolve/main/reallife_v30.safetensors?download=true'

# RealCartoon-XL - v5
RUN wget --content-disposition 'https://huggingface.co/reszkapl/reszkaplmodels/resolve/main/realcartoonXL_v5.safetensors?download=true'

RUN ls -lha

WORKDIR /home/appuser/stable-diffusion-webui/extensions

#Deforum ART
RUN git clone https://github.com/deforum-art/deforum-for-automatic1111-webui

#Additional Networks
RUN git clone https://github.com/kohya-ss/sd-webui-additional-networks

# Control Net
# https://github.com/Mikubill/sd-webui-controlnet
# https://github.com/Mikubill/sd-webui-controlnet/discussions/2039
RUN git clone https://github.com/Mikubill/sd-webui-controlnet

#Civitai Browser Plus
RUN git clone https://github.com/BlafKing/sd-civitai-browser-plus

#Animated Diff
RUN git clone https://github.com/continue-revolution/sd-webui-animatediff

# posex
RUN git clone https://github.com/hnmr293/posex

# stable-diffusion-webui-images-browser
RUN git clone https://github.com/AlUlkesh/stable-diffusion-webui-images-browser

# Reactor (Face Swap)
RUN git clone https://github.com/Gourieff/sd-webui-reactor-sfw.git

# Roop (Face Swap)
RUN git clone https://github.com/s0md3v/sd-webui-roop.git

# SD WebUI Aspect Ratio Helper
RUN git clone https://github.com/thomasasfk/sd-webui-aspect-ratio-helper.git


WORKDIR /home/appuser/stable-diffusion-webui

ADD webui-user.sh .

ENV IS_LOWVRAM true
ENV IS_MEDVRAM false
ENV USE_XFORMERS true
ENV USE_CUDA_118 true

RUN cat webui-user.sh

#RUN python3 -m venv venv
#ENV PATH="./venv/bin:$PATH"

RUN pip install --upgrade pip
RUN pip install -U xformers torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu128

EXPOSE 7860
ENTRYPOINT [ "./webui.sh" ]
