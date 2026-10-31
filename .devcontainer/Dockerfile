FROM mcr.microsoft.com/devcontainers/base:debian13

COPY --from=jdxcode/mise:2026.10.25 /usr/local/bin/mise /usr/local/bin/

# make sure mise is activated in both zsh and bash. Might be overriden by user's dotfiles.
RUN echo 'eval "$(mise activate bash)"' >> /home/vscode/.bashrc && \
    echo 'eval "$(mise activate zsh)"' >> /home/vscode/.zshrc
