FROM gcc:10.2.0 as builder

ARG SPACK_VERSION="v0.16.1"

RUN apt-get update -y && \
    apt-get -y install git lsb-release software-properties-common

RUN git clone https://github.com/spack/spack.git --branch ${SPACK_VERSION} /opt/spack && \
    echo "#!/bin/bash" > /etc/profile.d/spack.sh && \
    echo "SPACK_ROOT=/opt/spack" >> /etc/profile.d/spack.sh && \
    echo ". \${SPACK_ROOT}/share/spack/setup-env.sh" >> /etc/profile.d/spack.sh && \
    . /opt/spack/share/spack/setup-env.sh && \
    spack compiler find --scope=defaults

# Install hipfort
RUN mkdir /opt/spack-environment \
&&  (echo "spack:" \
&&   echo "  specs:" \
&&   echo "  - cmake@3.18.4 % gcc@10.2.0" \
&&   echo "  - hdf5@1.12.0+cxx+fortran~mpi % gcc@10.2.0" \
&&   echo "  concretization: together" \
&&   echo "  config:" \
&&   echo "    install_tree: /opt/software" \
&&   echo "  view: /opt/view") > /opt/spack-environment/spack.yaml

# Install the software, remove unnecessary deps
RUN . /opt/spack/share/spack/setup-env.sh && \ 
    cd /opt/spack-environment && spack env activate . && spack install --fail-fast && spack gc -y

# Modifications to the environment that are necessary to run
RUN . /opt/spack/share/spack/setup-env.sh && \ 
    cd /opt/spack-environment && \
    spack env activate --sh -d . >> /etc/profile.d/z10_spack_environment.sh

# Install cuda-compiler
RUN export DEBIAN_FRONTEND=noninteractive &&\
    wget https://developer.download.nvidia.com/compute/cuda/11.3.1/local_installers/cuda-repo-debian10-11-3-local_11.3.1-465.19.01-1_amd64.deb &&\
    dpkg -i cuda-repo-debian10-11-3-local_11.3.1-465.19.01-1_amd64.deb &&\
    apt-key add /var/cuda-repo-debian10-11-3-local/7fa2af80.pub &&\
    add-apt-repository contrib &&\
    apt-get update -y &&\
    apt-get -y install cuda &&\
    echo "#!/bin/bash" > /etc/profile.d/cuda.sh &&\
    echo "export PATH=/usr/local/cuda-11.3/bin\${PATH:+:\${PATH}}" >> /etc/profile.d/cuda.sh &&\
    echo "export LD_LIBRARY_PATH=/usr/local/cuda-11.3/lib64\${LD_LIBRARY_PATH:+:\${LD_LIBRARY_PATH}}" >> /etc/profile.d/cuda.sh

# Install rocm + hipfort
RUN . /etc/profile.d/z10_spack_environment.sh \
&&  wget -q -O - https://repo.radeon.com/rocm/rocm.gpg.key | apt-key add - \
&&  echo 'deb [arch=amd64] https://repo.radeon.com/rocm/apt/4.1/ xenial main' | tee /etc/apt/sources.list.d/rocm.list \
&& wget https://people.debian.org/~paravoid/python-all/unofficial-python-all.asc \
&& mv unofficial-python-all.asc /etc/apt/trusted.gpg.d \
&& echo "deb http://people.debian.org/~paravoid/python-all $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/python-all.list \
&& apt-get update -y \
&& apt-get install -y rocm-dev rocm-libs \
&& git clone https://github.com/ROCmSoftwarePlatform/hipfort.git /tmp/hipfort \
&& mkdir /tmp/hipfort/build \
&& cd /tmp/hipfort/build \
&& FC=$(which gfortran) cmake -DCMAKE_INSTALL_PREFIX=/opt/rocm /tmp/hipfort \
&& make -j install \
&& echo "PATH=\${PATH}:/opt/rocm/bin" >> /etc/profile.d/z10_spack_environment.sh


FROM gcc:10.2.0
COPY --from=builder /opt /opt
COPY --from=builder /etc/profile.d /etc/profile.d
COPY --from=builder /usr/local/cuda-11.3 /usr/local/cuda-11.3
COPY --from=builder /usr/local/cuda /usr/local/cuda

ENTRYPOINT ["/bin/bash", "--rcfile", "/etc/profile", "-l"]
