FROM gcc:10.2.0 as builder

ARG SPACK_VERSION="v0.16.1"
ARG HDF5_VERSION="1.12.0"

RUN apt-get update -y && \
    apt-get install git

RUN git clone https://github.com/spack/spack.git --branch ${SPACK_VERSION} /opt/spack && \
    echo "#!/bin/bash" > /etc/profile.d/spack.sh && \
    echo "SPACK_ROOT=/opt/spack" >> /etc/profile.d/spack.sh && \
    echo ". \${SPACK_ROOT}/share/spack/setup-env.sh" >> /etc/profile.d/spack.sh && \
    . /opt/spack/share/spack/setup-env.sh && \
    spack compiler find --scope=defaults

# Install HDF5
RUN mkdir /opt/spack-environment \
&&  (echo "spack:" \
&&   echo "  specs:" \
&&   echo "  - cmake@3.20.1 % gcc@10.2.0" \
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

FROM gcc:10.2.0
COPY --from=builder /opt /opt
COPY --from=builder /etc/profile.d /etc/profile.d
LABEL maintainer="joe@fluidnumerics.com"

ENTRYPOINT ["/bin/bash", "--rcfile", "/etc/profile", "-l"]
