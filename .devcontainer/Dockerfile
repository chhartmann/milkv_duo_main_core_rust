# Base image
ARG VARIANT=bookworm-slim
FROM debian:${VARIANT}
ENV DEBIAN_FRONTEND=noninteractive
ENV LC_ALL=C.UTF-8
ENV LANG=C.UTF-8

# Arguments
ARG CONTAINER_USER=milkv
ARG CONTAINER_GROUP=milkv
ARG GITHUB_TOKEN

# Install dependencies
RUN apt-get update \
    && apt-get install -y git curl llvm-dev libclang-dev clang unzip gdb-multiarch \
    libssl-dev pkg-config qemu-user file cpio git ssh make wget \
    && apt-get clean -y && rm -rf /var/lib/apt/lists/* /tmp/library-scripts

# Set users
RUN adduser --disabled-password --gecos "" ${CONTAINER_USER}
USER ${CONTAINER_USER}
WORKDIR /home/${CONTAINER_USER}

# Install rustup
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- \
    --default-toolchain none -y --profile minimal

# Update envs
ENV PATH=${PATH}:/home/${CONTAINER_USER}/.cargo/bin

# Set default toolchain
RUN rustup toolchain install nightly
RUN rustup component add rust-src --toolchain nightly
RUN rustup component add clippy
RUN rustup component add rustfmt


# Setup buildroot incl. toolchain with new uclibc
RUN git clone https://github.com/milkv-duo/duo-sdk.git
ENV PATH="$PATH:/home/milkv/duo-sdk/riscv64-linux-musl-x86_64/bin"

CMD [ "/bin/bash" ]