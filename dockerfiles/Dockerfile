FROM ubuntu:22.04

ARG username=user
ARG userid=1000
ARG groupid=100

SHELL ["/bin/bash", "-c"]

# Non-interactive installation mode
ENV DEBIAN_FRONTEND=noninteractive

# Set the locale
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

# Create non-root user for safety
ENV USER $username
ENV UID $userid
ENV GID $groupid
ENV HOME /home/$USER
ENV VENVNAME myenv

RUN adduser --disabled-password \
    --gecos "Non-root User" \
    --uid ${UID} \
    --gid ${GID} \
    --home $HOME \
    $USER

# COPY dependency files to a /tmp/ folder and install apt packages
COPY deps/apt-packages.txt /tmp/

RUN chown $UID:$GID /tmp/apt-packages.txt && \
    apt-get update > /dev/null && \
    xargs apt-get install --no-install-recommends --yes < /tmp/apt-packages.txt > /dev/null && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

COPY deps/environment.yml /tmp/
RUN chown $UID:$GID /tmp/environment.yml

# Switch to non-root user
USER ${USER}

# Create a workspace and a sample cpp project
ENV WSDIR  $HOME/my_ws
ENV SAMPLECPPDIR $WSDIR/sample-cpp
RUN mkdir -p $SAMPLECPPDIR
WORKDIR $SAMPLECPPDIR
COPY CMakeLists.txt $SAMPLECPPDIR/CMakeLists.txt
COPY src/main.cpp $SAMPLECPPDIR/src/main.cpp

# Prepare conda environment
ENV CONDA_DIR=$HOME/conda
ENV PATH=$CONDA_DIR/bin:$CONDA_DIR/envs/$VENVNAME/bin:$PATH
RUN  wget --no-hsts --quiet "https://github.com/conda-forge/miniforge/releases/latest/download/Mambaforge-$(uname)-$(uname -m).sh" -O ~/mambaforge_installer.sh && \
    sh ~/mambaforge_installer.sh -b -p ${CONDA_DIR} && \
    rm ~/mambaforge_installer.sh && \
    mamba clean --tarballs --index-cache --packages --yes && \
    find ${CONDA_DIR} -follow -type f -name '*.a' -delete && \
    find ${CONDA_DIR} -follow -type f -name '*.pyc' -delete && \
    mamba clean --force-pkgs-dirs --all --yes && \
    mamba create -n ${VENVNAME} python=3.9 && \
    echo ". ${CONDA_DIR}/etc/profile.d/conda.sh && conda activate ${VENVNAME}" >> ~/.bashrc

RUN mamba env update --name $VENVNAME \
                     --file /tmp/environment.yml && \
    mamba clean --tarballs --index-cache --packages --yes && \
    find ${CONDA_DIR} -follow -type f -name '*.a' -delete && \
    find ${CONDA_DIR} -follow -type f -name '*.pyc' -delete && \
    mamba clean --force-pkgs-dirs --all --yes




