FROM mcr.microsoft.com/devcontainers/python:1-3.12

# Install uv
RUN pip install uv

# Install make and other common tools
RUN apt-get update && apt-get install -y --no-install-recommends \
    make \
    && rm -rf /var/lib/apt/lists/*
