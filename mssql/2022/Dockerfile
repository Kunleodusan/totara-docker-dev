# mssql-agent-fts-ha-tools
# Maintainers: Microsoft Corporation (twright-msft on GitHub)
# GitRepo: https://github.com/Microsoft/mssql-docker

FROM --platform=linux/amd64 ubuntu:22.04

# Install Server FTS & HA
RUN apt-get update && \
    export DEBIAN_FRONTEND=noninteractive && \
    apt-get install -y \
        gnupg && \
    apt-get install -y \
        curl \
        locales \
        apt-transport-https && \
    curl https://packages.microsoft.com/keys/microsoft.asc | tee /etc/apt/trusted.gpg.d/microsoft.asc && \
    curl -fsSL https://packages.microsoft.com/config/ubuntu/22.04/mssql-server-2022.list | tee /etc/apt/sources.list.d/mssql-server-2022.list && \
    curl https://packages.microsoft.com/config/ubuntu/22.04/prod.list | tee /etc/apt/sources.list.d/mssql-release.list && \
    export DEBIAN_FRONTEND=noninteractive && \
    apt-get update && \
    ACCEPT_EULA=Y apt-get install -y \
        mssql-server \
        mssql-server-ha \
        mssql-server-fts \
        mssql-tools18 \
        unixodbc-dev && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists

# Link mssql-tools18 to mssql-tools for backward compatibility
RUN ln -s /usr/bin/mssql-tools18 /usr/bin/mssql-tools

# Copy the config across
RUN mkdir /custom
COPY custom /custom/

RUN chmod +x /custom/*.sh

# Run SQL Server process
ENTRYPOINT ["/custom/entrypoint.sh"]