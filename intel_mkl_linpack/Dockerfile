FROM ubuntu:16.04

RUN sed -e s:archive.ubuntu.com:se.archive.ubuntu.com:g -i /etc/apt/sources.list
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install -y wget

RUN adduser --gecos "" --disabled-password burn

USER burn:burn
RUN cd /home/burn/ \
 && wget http://registrationcenter-download.intel.com/akdlm/irc_nas/9752/l_mklb_p_2017.2.015.tgz \
 && tar xzf l_mklb_p_2017.2.015.tgz
 # Note this is not an official checksum
# && echo "c09246c62da66a4d84506a2070586eb44033ade9649b0c41b913022f01d1aa30  gpu_burn-0.7.tar.gz" \
#    | sha256sum -c --strict - \
WORKDIR "/home/burn/l_mklb_p_2017.2.015/benchmarks_2017/linux/mkl/benchmarks/linpack"
ENTRYPOINT ["./runme_xeon64"]
