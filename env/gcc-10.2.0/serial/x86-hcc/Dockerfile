FROM gcc:10.2.0 as builder

ARG SPACK_VERSION="v0.16.1"

RUN apt-get update -y && \
    apt-get install git



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
    spack env activate --sh -d . >> /etc/profile.d/z10_spack_environment.sh && \
    echo "PATH=\${PATH}:/opt/rocm/bin"

# Install rocm + hipfort
RUN . /etc/profile.d/z10_spack_environment.sh \
&&  wget -q -O - https://repo.radeon.com/rocm/rocm.gpg.key | apt-key add - \
&&  echo 'deb [arch=amd64] https://repo.radeon.com/rocm/apt/debian/ xenial main' | tee /etc/apt/sources.list.d/rocm.list \
&&  apt-get update -y \
&&  apt-get install rocm-dev rocm-libs \
&& git clone https://github.com/ROCmSoftwarePlatform/hipfort.git /tmp/hipfort \
&& mkdir /tmp/hipfort/build \
&& cd /tmp/hipfort/build \
&& FC=$(which gfortran) cmake -DCMAKE_INSTALL_PREFIX=/tmp/apps/hipfort /tmp/hipfort \
&& make -j install

FROM gcc:10.2.0
COPY --from=builder /opt /opt
COPY --from=builder /etc/profile.d /etc/profile.d

ENTRYPOINT ["/bin/bash", "--rcfile", "/etc/profile", "-l"]
