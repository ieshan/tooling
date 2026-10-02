FROM node:26.10.0-alpine3.24

# Pin uv; bump deliberately (check github.com/astral-sh/uv/releases)
COPY --from=ghcr.io/astral-sh/uv:0.12.22 /uv /uvx /usr/local/bin/

ENV NPM_CONFIG_PREFIX=/opt/npm-global \
    UV_PYTHON_INSTALL_DIR=/opt/python \
    UV_PYTHON_BIN_DIR=/usr/local/bin \
    HOME=/home/node
ENV PATH=$NPM_CONFIG_PREFIX/bin:$PATH

# Latest stable Python known to this uv release, exposed as python/python3
RUN uv python install --default --compile-bytecode \
    && python3 --version \
    && python --version

# command-code (unchanged)
RUN mkdir -p $NPM_CONFIG_PREFIX \
    && npm install -g command-code@latest \
    && mkdir -p /home/node/.commandcode \
    && chown -R node:node /home/node /opt/npm-global /opt/python

USER node
WORKDIR /work
ENTRYPOINT ["cmd"]

