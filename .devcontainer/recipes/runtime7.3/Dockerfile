FROM ubuntu:18.04

SHELL ["/bin/bash", "-c"]

# This Dockerfile adds a non-root user with sudo access. Use the "remoteUser"
# property in devcontainer.json to use it. On Linux, the container user's GID/UIDs
# will be updated to match your local UID/GID (when using the dockerFile property).
# See https://aka.ms/vscode-remote/containers/non-root-user for details.
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Configure apt
ENV DEBIAN_FRONTEND=noninteractive

# Localization
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

RUN apt-get update --fix-missing && \
    apt-get install -y wget bzip2 ca-certificates curl git software-properties-common && \
    apt-get install -y gcc g++ build-essential openjdk-8-jdk openjdk-8-jre && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN add-apt-repository -y ppa:deadsnakes/ppa
RUN apt install -y python3.7-dev \
    python3.7-full 

RUN python3.7 -m ensurepip

RUN pip3.7 install --upgrade pip

#uninstall spark 
RUN pip3.7 uninstall pyspark

#install 
RUN pip3.7 install -U "databricks-connect==7.3.*" six pep8 pylint ipykernel ipython

# THIS ONE COULD BE BECOME REQUIREMENTS
#COPY environments/environment${DBCVER}.yml .
#RUN pip install --upgrade pip \
#    && conda env create -f environment${DBCVER}.yml \
#   && echo '{}' > /root/.databricks-connect



# VSCode DevContainers
RUN apt-get update \
    && apt-get -y install --no-install-recommends apt-utils dialog 2>&1 \
    # Create a non-root user to use if preferred - see https://aka.ms/vscode-remote/containers/non-root-user.
    && groupadd --gid $USER_GID $USERNAME \
    && useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME \
    # [Optional] Add sudo support for the non-root user
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME\
    && chmod 0440 /etc/sudoers.d/$USERNAME \
    # Verify git, needed tools installed
    && apt-get -y install git iproute2 procps curl lsb-release sudo

# Use a well-known password for root
RUN echo 'root:Docker!' | chpasswd

ENV JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
ENV JRE_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre
ENV SPARK_HOME=/usr/local/lib/python3.7/dist-packages/pyspark

RUN python3.7 --version
RUN java -version

# Clean up
RUN apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

ENV DEBIAN_FRONTEND=dialog

# Vital workaround to make databricks-connect work
RUN echo '{}' > ~/.databricks-connect  #source: https://forums.databricks.com/questions/21536/databricks-connect-configuration-not-possible-with.html
RUN echo '{}' > /home/vscode/.databricks-connect
