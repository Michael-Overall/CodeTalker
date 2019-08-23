# Adapted from a Dockerfile created by Andy Wong: https://hub.docker.com/r/binkybong/speech-recognition/
FROM centos:centos7 

# install and update CentOS packages
RUN \
yum install epel-release -y &&\
yum clean all && yum update -y
#install python and audio packages
RUN \
yum install portaudio-devel python34-pip mlocate bash-completion git wget sox libtool autoconf bison swig python34-devel python34-numpy python34-scipy python-devel doxygen alsa-utils alsa-lib-devel pulseaudio-libs-devel make -y

#more configuration: alias 'pip' as pip3.4
RUN \
echo 'alias pip=pip3.4' >> ~/.bashrc
#install python packages using python's package manager (port audio bindings and speech to text)

RUN \
#can't use our alias yet, since .bashrc is read on login
pip3.4 install pyaudio &&\
pip3.4 install SpeechRecognition &&\
chmod -R ugo+rX /lib/python2.7/site-packages/ &&\
chmod -R ugo+rX /lib/python3.4/site-packages/


RUN \
updatedb

#Nodejs dependencies
RUN \
yum install -y gcc gcc-c++
##conflating with && will cause docker to claim CURL is not a command for some reason
RUN \
curl -sL https://rpm.nodesource.com/setup_10.x | bash - &&\
yum install -y nodejs

#THIS WILL BREAK YUM IF NOT ADDRESSED BECAUSE IT RELIES ON PYTHON2!!
#ensure that CentOS uses the correct version of python (use altneratives to alias python3.4 as current version of python)
RUN \
alternatives --install /usr/bin/python python /usr/bin/python2 1; \
alternatives --install /usr/bin/python python /usr/bin/python3.4 2
#fix the yum script to run specifically using python2 since we just aliased python to be 3.4:
RUN \
sed -i 's/python/python2/' /usr/bin/yum > /usr/bin/yum

#create a directory to mount script directory later with "docker run -v"
RUN \ 
mkdir -p /src/micListener/ 

WORKDIR \
 /src/micListener