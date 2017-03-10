FROM ubuntu:16.04

RUN sed -e s:archive.ubuntu.com:se.archive.ubuntu.com:g -i /etc/apt/sources.list
RUN apt-get update \
 && apt-get upgrade -y \
 && apt-get install -y build-essential git ca-certificates libssl-dev \
                       curl

RUN adduser --gecos "" --disabled-password builder
USER builder:builder
RUN cd /home/builder && \
    curl -sSf https://static.rust-lang.org/rustup/dist/x86_64-unknown-linux-gnu/rustup-init > rustup-init && \
    chmod +x rustup-init && \
    ./rustup-init -y --no-modify-path && \
    rm rustup-init && \
    PATH="/home/builder/.cargo/bin/:$PATH" && \
    export USER="builder" && \

    cd /home/builder/ && \
    cargo init --bin fasta && \
    cd /home/builder/fasta && \
    curl -sSf http://masv.se/bench/fasta_main.rs > src/main.rs && \
    echo "num_cpus = \"1.3.0\"" >> Cargo.toml && \
    cargo build --release && \
    ./target/release/fasta 25000000 > /home/builder/input25000000.txt && \

    cd /home/builder && \
    cargo init --bin knucleotides && \
    cd /home/builder/knucleotides && \
    curl -sSf http://masv.se/bench/knucleotides_main.rs > src/main.rs && \
    echo "futures = \"0.1.10\"" >> Cargo.toml && \
    echo "futures-cpupool = \"0.1.3\"" >> Cargo.toml && \
    echo "ordermap = \"0.2.8\"" >> Cargo.toml && \
    cargo build --release
USER root:root
