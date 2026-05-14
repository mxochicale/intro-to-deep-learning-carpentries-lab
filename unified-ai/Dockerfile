# syntax=docker/dockerfile:1.7
FROM ghcr.io/ucl-arc-unified-ai/kubeflow-notebook-images-pytorch-default:v1.0.20

USER root
RUN apt-get update && apt-get install -y graphviz && rm -rf /var/lib/apt/lists/*

USER jovyan


WORKDIR /workspace

RUN /opt/conda/bin/python -m pip install --no-cache-dir \
    tensorflow \
    tensorboard \
    seaborn \
    keras-tuner

