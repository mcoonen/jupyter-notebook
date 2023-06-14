FROM jupyter/base-notebook:ubuntu-20.04

USER root

# Install basic OS packages
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
    curl \
    gnupg \
    nano

# Add Microsoft apt repo for MS SQL
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - 
RUN curl https://packages.microsoft.com/config/ubuntu/22.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

RUN cat /etc/apt/sources.list.d/mssql-release.list

# Install required OS packages
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive \ 
    ACCEPT_EULA=Y \
    apt-get install -y \
    msodbcsql17 mssql-tools \
    unixodbc

# Reduce the security level for compatiblity for MS SQL clusters on older Ciphersuites
# Ubuntu < 22.04 SECLEVEL=1, Ubuntu >= 22.04 SECLEVEL=0
COPY ./openssl.cnf /etc/ssl/openssl.cnf

USER jovyan

# Install some common python packages
# TODO: Think about a better way. E.g. looping over the requirements.txt files in the different notebook folders to install the required packages.
COPY ./requirements.txt /tmp/requirements.txt
RUN pip install -r /tmp/requirements.txt

