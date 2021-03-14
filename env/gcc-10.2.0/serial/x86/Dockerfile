FROM gcc:10.2.0 as builder

ARG SPACK_VERSION="v0.16.0"

RUN apt-get update -y && \
    apt-get install git

RUN git clone https://github.com/spack/spack.git --branch ${SPACK_VERSION} /opt/spack && \
    echo "#!/bin/bash" > /etc/profile.d/spack.sh && \
    echo "SPACK_ROOT=/opt/spack" >> /etc/profile.d/spack.sh && \
    echo ". \${SPACK_ROOT}/share/spack/setup-env.sh" >> /etc/profile.d/spack.sh && \
    . /opt/spack/share/spack/setup-env.sh && \
    spack compiler find --scope=defaults


FROM gcc:10.2.0
COPY --from=builder /opt /opt
COPY --from=builder /etc/profile.d /etc/profile.d
ENTRYPOINT ["/bin/bash", "--rcfile", "/etc/profile", "-l"]
