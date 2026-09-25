FROM node:24.20.0-bullseye

RUN useradd -m -s /bin/bash vscode
RUN mkdir -p /workspaces && chown -R vscode:vscode /workspaces
WORKDIR /workspaces
