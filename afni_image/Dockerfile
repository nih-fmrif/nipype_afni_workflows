FROM ubuntu:17.04

ARG DEBIAN_FRONTEND=noninteractive

#----------------------------
# Install common dependencies
#----------------------------
RUN apt-get update -qq && apt-get install -yq --no-install-recommends bzip2 ca-certificates curl unzip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

#-------------------------------------------------
# Install Miniconda, and set up Python environment
#-------------------------------------------------
ENV PATH=/opt/miniconda/envs/default/bin:$PATH
RUN echo "Downloading Miniconda installer ..." \
    && curl -sSL -o miniconda.sh https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh \
    && bash miniconda.sh -b -p /opt/miniconda \
    && rm -f miniconda.sh \
    && /opt/miniconda/bin/conda config --add channels conda-forge \
    && /opt/miniconda/bin/conda create -y -q -n default python=3.5.1 \
    	traits \
    && /opt/miniconda/bin/conda create -y -q -n py27 python=2.7 pyqt=4 \
    && conda clean -y --all \
    && pip install -U -q --no-cache-dir pip \
    && pip install -q --no-cache-dir \
    	https://github.com/nipy/nipype/archive/master.tar.gz 

#---------------------------
# Add NeuroDebian repository
#---------------------------
RUN apt-get update -qq && apt-get install -yq --no-install-recommends dirmngr \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    && curl -sSL http://neuro.debian.net/lists/zesty.us-nh.libre \
    > /etc/apt/sources.list.d/neurodebian.sources.list \
    && apt-key adv --recv-keys --keyserver hkp://p80.pool.sks-keyservers.net:80 0xA5D32F012649A5A9 \
    && apt-get update \
    && apt-get purge -y --auto-remove dirmngr

# Install NeuroDebian packages
RUN apt-get update -qq && apt-get install -yq --no-install-recommends dcm2niix \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

#---------------------------
# Install AFNI
#---------------------------
RUN apt-get update -qq \
    && apt-get install -yq --no-install-recommends tcsh xfonts-base     \
                        gsl-bin netpbm gnome-tweak-tool   \
                        libjpeg62 xvfb xterm vim curl  \
                        libglu1-mesa-dev libglw1-mesa     \
                        libxm4 build-essential 

RUN curl -O https://afni.nimh.nih.gov/pub/dist/bin/linux_ubuntu_16_64/@update.afni.binaries \
    && tcsh @update.afni.binaries -package linux_ubuntu_16_64  -do_extras 

# Install R packages
RUN export R_LIBS=$HOME/R \
    && mkdir $R_LIBS \
    && echo 'export R_LIBS=$HOME/R' >> ~/.bashrc \
    && echo 'export PATH=$HOME/abin/:$PATH' >> ~/.bashrc \
    && . ~/.bashrc \
    && curl -O https://raw.githubusercontent.com/Shotgunosine/afni_add_rcran_ubuntu/master/@add_rcran_ubuntu.tcsh \
    && tcsh @add_rcran_ubuntu.tcsh \
    && rPkgsInstall -pkgs ALL 

# Configure AFNI environment and make a place for the tutorial data to land
RUN . ~/.bashrc \ 
    && cp $HOME/abin/AFNI.afnirc $HOME/.afnirc \
    && suma -update_env \
    && mkdir ~\CD
VOLUME ~\CD
#--------------------------
# User-defined instructions
#--------------------------

RUN echo "Hello, World"

