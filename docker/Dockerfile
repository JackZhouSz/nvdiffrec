# Copyright (c) 2020-2022 NVIDIA CORPORATION & AFFILIATES. All rights reserved. 
#
# NVIDIA CORPORATION, its affiliates and licensors retain all intellectual
# property and proprietary rights in and to this material, related
# documentation and any modifications thereto. Any use, reproduction, 
# disclosure or distribution of this material and related documentation 
# without an express license agreement from NVIDIA CORPORATION or 
# its affiliates is strictly prohibited.

ARG BASE_IMAGE=nvcr.io/nvidia/pytorch:22.07-py3
FROM $BASE_IMAGE

RUN apt-get update -yq --fix-missing \
 && DEBIAN_FRONTEND=noninteractive apt-get install -yq --no-install-recommends \
    pkg-config \
    libglvnd0 \
    libgl1 \
    libglx0 \
    libegl1 \
    libgles2 \
    libglvnd-dev \
    libgl1-mesa-dev \
    libegl1-mesa-dev \
    libgles2-mesa-dev \
    cmake \
    curl

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

# nvidia-container-runtime
ENV NVIDIA_VISIBLE_DEVICES all
ENV NVIDIA_DRIVER_CAPABILITIES compute,utility,graphics

# Default pyopengl to EGL for good headless rendering support
ENV PYOPENGL_PLATFORM egl

COPY 10_nvidia.json /usr/share/glvnd/egl_vendor.d/10_nvidia.json

# additional libraries
RUN pip install ninja imageio imageio-ffmpeg xatlas gdown

# install nvdiffrast
RUN pip install git+https://github.com/NVlabs/nvdiffrast@253ac4f

# HDR image support (FreeImage) — pin URL + verify SHA256 (imageio_download_bin has no hash check)
ARG FREEIMAGE_URL=https://github.com/imageio/imageio-binaries/raw/4483dd4d18eeedbc16da8108fc4a2f209e8d0358/freeimage/libfreeimage-3.16.0-linux64.so
ARG FREEIMAGE_SHA256=8ba7be1e5d2bc8ffc94d111cb05f7de3dbbfd4af4d502a54eb22c4708021897c
RUN mkdir -p /root/.imageio/freeimage \
 && curl -fsSL -o /root/.imageio/freeimage/libfreeimage-3.16.0-linux64.so "${FREEIMAGE_URL}" \
 && echo "${FREEIMAGE_SHA256}  /root/.imageio/freeimage/libfreeimage-3.16.0-linux64.so" | sha256sum -c -
