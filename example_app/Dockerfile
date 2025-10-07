FROM python:3.13-slim AS c2pie_base

ENV PIP_NO_CACHE_DIR=off \
  PIP_DISABLE_PIP_VERSION_CHECK=on \
  PIP_DEFAULT_TIMEOUT=100 \
  POETRY_VERSION=2.2.0

WORKDIR /example_app

RUN apt-get update && apt-get install -y --no-install-recommends \
    curl ca-certificates build-essential ssh libffi-dev libssl-dev
    
RUN pip install "poetry>=$POETRY_VERSION"

ENV RUSTUP_HOME=/home/vscode/.rustup \
    CARGO_HOME=/home/vscode/.cargo \
    PATH=/home/vscode/.cargo/bin:/home/vscode/.local/bin:$PATH
RUN curl -sSf https://sh.rustup.rs | sh -s -- -y --default-toolchain stable && \
    cargo install c2patool --locked

COPY c2pie ./c2pie
COPY pyproject.toml poetry.lock .
COPY example_app .


RUN poetry config virtualenvs.create false \
  && poetry lock && poetry install --without dev --no-interaction --no-ansi 

FROM c2pie_base AS c2pie_with_jupyter

RUN pip install jupyterlab