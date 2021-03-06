# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# This Dockerfile in thirdpart_containers/jenkins/dockerfile dir is used to create an image with
# AdoptOpenJDK jdk binary installed. Basic test dependent executions
# are installed during the building process.
#
# Build example: `docker build -t adoptopenjdk-jenkins-test .`
#
# This Dockerfile builds image based on adoptopenjdk/openjdk8:latest.
# If you want to build image based on other images, please use
# `--build-arg list` to specify your base image
#
# Build example: `docker build --build-arg IMAGE_NAME=<image_name> --build-arg IMAGE_VERSION=<image_version >-t adoptopenjdk-jenkins-test .`

ARG SDK=openjdk8
ARG IMAGE_NAME=adoptopenjdk/$SDK
ARG IMAGE_VERSION=latest

FROM $IMAGE_NAME:$IMAGE_VERSION

# Install test dependent executable files
RUN apt-get update \
	&& apt-get -y install \
	ant \
	apt-transport-https \
	ca-certificates \
	dirmngr \
	curl \
	git \
	make \
	unzip \
	vim \
	wget

# Install Maven
RUN mkdir -p /usr/share/maven \
	&& cd /usr/share/maven \
	&& wget http://www-us.apache.org/dist/maven/maven-3/3.5.3/binaries/apache-maven-3.5.3-bin.zip \
	&& unzip apache-maven-3.5.3-bin.zip \
	&& rm -rf apache-maven-3.5.3-bin.zip \
	&& ln -s /usr/share/maven/apache-maven-3.5.3/bin/mvn /usr/bin/mvn \
	&& cd /

VOLUME ["/java"]
ENV JAVA_HOME=/java \
    PATH=/java/bin:$PATH \
    JAVA_TOOL_OPTIONS="-Dfile.encoding=UTF8"

# This is the main script to run jenkins tests
COPY ./dockerfile/jenkins-test.sh /jenkins-test.sh

# Clone jenkins src to /jenkins
WORKDIR /
RUN pwd
RUN git clone https://github.com/jenkinsci/jenkins.git

ENTRYPOINT ["/bin/bash", "/jenkins-test.sh"]
CMD ["--version"]
