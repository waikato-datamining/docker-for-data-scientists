ARG PYTORCH="1.6.0"
ARG CUDA="10.1"
ARG CUDNN="7"

FROM pytorch/pytorch:${PYTORCH}-cuda${CUDA}-cudnn${CUDNN}-devel

RUN pip --no-cache install matplotlib

COPY test.py /opt/test/test.py

WORKDIR /opt/test
