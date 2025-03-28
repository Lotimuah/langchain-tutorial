# Use the base image
FROM python:3.12-slim-bookworm

# Development(dev) or production(prd) image
ARG ENV=dev
ENV ENV=${ENV}

# Set project directory
ENV PROJECT_ROOT=/app
ENV PYTHONPATH=${PROJECT_ROOT}
WORKDIR ${PROJECT_ROOT}

# Set the timezone to Asia/Seoul
ENV TZ=Asia/Seoul
RUN ln -snf /usr/share/zoneinfo/Asia/Seoul /etc/localtime

# Create non-root user
ARG UID=1000
ARG GID=1000
RUN groupadd -g ${GID} appuser && \
    useradd -u ${UID} -g ${GID} -ms /bin/bash appuser

# Install system dependencies
RUN apt-get update -qqy && \
    apt-get install -y --no-install-recommends \
        git build-essential && \
    apt-get autoremove && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf ~/.cache

# Copy requirements first to leverage cache
COPY requirements.txt .
RUN pip install --no-cache-dir --upgrade -r requirements.txt

# Switch to non-root user
RUN chown -R appuser:appuser ${PROJECT_ROOT}
USER appuser
