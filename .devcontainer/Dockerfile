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
    libssl-dev pkg-config qemu-user file cpio git ssh make wget xz-utils \
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

# Set rust toolchain
RUN rustup toolchain install nightly
RUN rustup target add riscv64gc-unknown-linux-musl
RUN rustup component add clippy
RUN rustup component add rustfmt


# Install riscv64 toolchain
RUN wget https://toolchains.bootlin.com/downloads/releases/toolchains/riscv64-lp64d/tarballs/riscv64-lp64d--musl--stable-2024.05-1.tar.xz
RUN xz --decompress riscv64-lp64d--musl--stable-2024.05-1.tar.xz
RUN tar -xf riscv64-lp64d--musl--stable-2024.05-1.tar
RUN rm riscv64-lp64d--musl--stable-2024.05-1.tar
RUN ln -s $PWD/riscv64-lp64d--musl--stable-2024.05-1/riscv64-buildroot-linux-musl/sysroot/lib/ld-musl-riscv64.so.1 $PWD/riscv64-lp64d--musl--stable-2024.05-1/riscv64-buildroot-linux-musl/sysroot/lib/ld-musl-riscv64v0p7_xthead.so.1
CMD [ "/bin/bash" ]
