FROM python:3.12-slim

# Install uv and gemini-cli
RUN pip install uv gemini-cli

# Install make and other common tools
RUN apt-get update && apt-get install -y --no-install-recommends \
    make \
    curl \
    git \
    && rm -rf /var/lib/apt/lists/*
