FROM python:latest
# Homebrew に必要なパッケージ + sudo
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential curl file git procps sudo \
    && rm -rf /var/lib/apt/lists/*
# 非rootユーザー作成(sudoはパスワードなしで許可)
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=1000
RUN groupadd --gid ${USER_GID} ${USERNAME} \
    && useradd --uid ${USER_UID} --gid ${USER_GID} -m -s /bin/bash ${USERNAME} \
    && echo "${USERNAME} ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/${USERNAME} \
    && chmod 0440 /etc/sudoers.d/${USERNAME}

# install pre-commit
COPY requirements.txt /requirements.txt
RUN pip install -r /requirements.txt --no-cache-dir
# /app を非rootユーザーが書き込めるようにする
RUN mkdir -p /app && chown ${USERNAME}:${USERNAME} /app

# ここから非root
USER ${USERNAME}
WORKDIR /home/${USERNAME}
RUN git clone https://github.com/togra1997/neovim_tmux_linux.git \
    &&  ./neovim_tmux_linux/setup.sh
WORKDIR /app
