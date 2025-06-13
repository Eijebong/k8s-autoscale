FROM ghcr.io/astral-sh/uv:bookworm-slim

# Install python in a shared location
ENV UV_PYTHON_INSTALL_DIR=/python

ARG PYTHON_VERSION
RUN uv python install $PYTHON_VERSION

RUN groupadd --gid 10001 app && \
    useradd -g app --uid 10001 --shell /usr/sbin/nologin --create-home --home-dir /app app

COPY . /app

WORKDIR /app

# %include docker.d
COPY topsrcdir/docker.d/healthcheck /bin/healthcheck
COPY topsrcdir/docker.d/init.sh /app/bin/init.sh

# %include configs
# %include MANIFEST.in
# %include README.rst
# %include pyproject.toml
# %include src
COPY topsrcdir/configs/ /app/configs/
COPY topsrcdir/MANIFEST.in /app/
COPY topsrcdir/README.rst /app/
COPY topsrcdir/pyproject.toml /app/
COPY topsrcdir/src/ /app/src/
RUN uv venv
RUN uv pip install .

RUN chown -R app:app /app

USER app
CMD ["/app/bin/init.sh"]
