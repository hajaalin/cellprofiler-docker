FROM ubuntu:20.04

ARG VERSION=4.2.x

# set timezone
ENV TZ=Europe/Helsinki
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# add apt mirrors
RUN apt-get update && apt-get install -y ca-certificates
RUN echo "deb mirror://mirrors.ubuntu.com/mirrors.txt focal main restricted universe multiverse " > /etc/apt/sources.list && \
    echo "deb mirror://mirrors.ubuntu.com/mirrors.txt focal-updates main restricted universe multiverse" >> /etc/apt/sources.list && \
    echo "deb mirror://mirrors.ubuntu.com/mirrors.txt focal-security main restricted universe multiverse" >> /etc/apt/sources.list

# upgrade system
RUN apt-get update && apt-get upgrade -y

# install dependencies
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y \
    build-essential \
    default-libmysqlclient-dev \
    git \
    libgtk-3-dev \
    libnotify-dev \
    libsdl2-dev \
    libzmq3-dev \
    locales \
    openjdk-11-jdk-headless \
    python3-pip \
    wget

# set locale
RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && locale-gen
ENV LANG en_US.UTF-8  
ENV LANGUAGE en_US:en  
ENV LC_ALL en_US.UTF-8 

# set java paths
ENV JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
ENV PATH=$PATH:/home/ubuntu/.local/bin

# add env for java options
ENV JAVA_OPTS=""

# install Python dependencies
RUN wget https://extras.wxpython.org/wxPython4/extras/linux/gtk3/ubuntu-20.04/wxPython-4.1.0-cp38-cp38-linux_x86_64.whl
RUN pip3 install \
        numpy==1.24.3 \
        wxPython-4.1.0-cp38-cp38-linux_x86_64.whl

# fetch Cellprofiler
RUN git clone --depth 1 --branch ${VERSION} https://github.com/CellProfiler/CellProfiler.git

# install CellProfiler
RUN cd /CellProfiler && pip3 install .

# set entrypoint
ENTRYPOINT ["cellprofiler"]
