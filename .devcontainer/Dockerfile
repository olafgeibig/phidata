ARG VARIANT=3-bookworm
FROM mcr.microsoft.com/vscode/devcontainers/python:${VARIANT}

RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    # Remove imagemagick due to https://security-tracker.debian.org/tracker/CVE-2019-10131
    && apt-get purge -y imagemagick imagemagick-6-common 

# Temporary: Upgrade python packages due to https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2022-40897 and https://github.com/advisories/GHSA-2mqj-m65w-jghx
# They are installed by the base image (python) which does not have the patch.
RUN python3 -m pip install --upgrade \
    setuptools==69.0.3 \
    gitpython==3.1.41

RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
    postgresql-client

RUN pip3 --disable-pip-version-check --no-cache-dir install isort && \
    pip3 --disable-pip-version-check --no-cache-dir install poetry && \
    pip install --upgrade pip

# Copy poetry config
COPY .devcontainer/pypoetry_config.toml /home/vscode/.config/pypoetry/config.toml

# RUN chown -R vscode:vscode /home/vscode/.config && \
#     /usr/local/py-utils/bin/poetry completions bash > /etc/bash_completion.d/poetry.bash-completion && \
#     python -m venv /workspace/.venv --prompt feria && \
#     chown -R vscode:vscode /workspace/.venv