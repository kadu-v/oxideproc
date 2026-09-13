FROM nvidia/cuda:13.0.0-devel-ubuntu24.04

ARG DEBIAN_FRONTEND=noninteractive
ARG USERNAME=ubuntu
ARG CUDA_OXIDE_REV=6abfaa091e29a6275c1943895bfbc97efa306e98
ARG RUST_TOOLCHAIN=nightly-2026-08-28

ENV CUDA_HOME=/usr/local/cuda \
    CUDA_PATH=/usr/local/cuda \
    CUDA_TOOLKIT_PATH=/usr/local/cuda \
    CUDA_OXIDE_LLC=/usr/bin/llc-21 \
    LIBCLANG_PATH=/usr/lib/llvm-21/lib \
    LLVM_CONFIG_PATH=/usr/bin/llvm-config-21 \
    RUSTUP_HOME=/home/${USERNAME}/.rustup \
    CARGO_HOME=/home/${USERNAME}/.cargo \
    PATH=/home/${USERNAME}/.cargo/bin:/usr/lib/llvm-21/bin:/usr/local/cuda/bin:${PATH}

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN apt-get update \
    && apt-get install -y --no-install-recommends ca-certificates curl g++ gcc git gnupg \
        libc6-dev make pkg-config python3 python3-pip sudo xz-utils \
    && curl -fsSL https://apt.llvm.org/llvm-snapshot.gpg.key \
        | gpg --dearmor -o /usr/share/keyrings/apt.llvm.org.gpg \
    && echo "deb [signed-by=/usr/share/keyrings/apt.llvm.org.gpg] https://apt.llvm.org/noble/ llvm-toolchain-noble-21 main" \
        > /etc/apt/sources.list.d/llvm-toolchain-noble-21.list \
    && apt-get update \
    && apt-get install -y --no-install-recommends clang-21 libclang-common-21-dev \
        lld-21 llvm-21 llvm-21-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && echo "${USERNAME} ALL=(root) NOPASSWD:ALL" > "/etc/sudoers.d/${USERNAME}" \
    && chmod 0440 "/etc/sudoers.d/${USERNAME}" \
    && mkdir -p /workspaces/oxideproc \
    && chown -R "${USERNAME}:${USERNAME}" /workspaces/oxideproc

USER ${USERNAME}

RUN curl --proto '=https' --tlsv1.2 -fsSL https://sh.rustup.rs \
        | sh -s -- -y --profile minimal --default-toolchain none \
    && rustup toolchain install "${RUST_TOOLCHAIN}" --profile minimal \
        --component rust-src,rustc-dev,rust-analyzer,clippy,rustfmt,llvm-tools \
    && rustup default "${RUST_TOOLCHAIN}" \
    && cargo +"${RUST_TOOLCHAIN}" install --locked \
        --git https://github.com/NVlabs/cuda-oxide.git \
        --rev "${CUDA_OXIDE_REV}" cargo-oxide

WORKDIR /workspaces/oxideproc

CMD ["/bin/bash"]
