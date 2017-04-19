FROM nvidia/cuda:8.0-cudnn6-devel-ubuntu16.04

RUN sed -e s:archive.ubuntu.com:se.archive.ubuntu.com:g -i /etc/apt/sources.list
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install -y build-essential wget

RUN adduser --gecos "" --disabled-password burn

USER burn:burn
ENV PATH="/usr/local/nvidia/bin:/usr/local/cuda/bin:${PATH}"
RUN cd /home/burn/ \
 && wget http://wili.cc/blog/entries/gpu-burn/gpu_burn-0.7.tar.gz \
 # Note this is not an official checksum
 && echo "c09246c62da66a4d84506a2070586eb44033ade9649b0c41b913022f01d1aa30  gpu_burn-0.7.tar.gz" \
    | sha256sum -c --strict - \
 && mkdir gpu_burn-0.7 \
 && cd /home/burn/gpu_burn-0.7 \
 && tar xvzf /home/burn/gpu_burn-0.7.tar.gz \
 && rm /home/burn/gpu_burn-0.7.tar.gz \
 && make
WORKDIR "/home/burn/gpu_burn-0.7"
ENTRYPOINT ["./gpu_burn"]
