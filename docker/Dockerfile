# Copyright 2014 Google Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM google/debian:wheezy
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get -q update && \
    apt-get -y -q --no-install-recommends install ca-certificates curl openjdk-7-jre-headless wget && \
    apt-get clean && \
    rm /var/lib/apt/lists/*_*

RUN mkdir -p /var/lib/jetty
RUN curl -0 http://repo1.maven.org/maven2/org/eclipse/jetty/jetty-distribution/9.2.5.v20141112/jetty-distribution-9.2.5.v20141112.tar.gz | tar -C /var/lib/jetty --strip-components=1 -zx

# Managed VM related jars:
RUN curl -0 http://repo1.maven.org/maven2/com/google/appengine/appengine-api-1.0-sdk/1.9.17/appengine-api-1.0-sdk-1.9.17.jar -o /var/lib/jetty/lib/ext/appengine-api-1.0-sdk-1.9.17.jar
RUN curl -0 https://oss.sonatype.org/content/repositories/google-releases/com/google/appengine/appengine-jetty-managed-runtime/1.9.17/appengine-jetty-managed-runtime-1.9.17.jar -o /var/lib/jetty/lib/ext/appengine-jetty-managed-runtime-1.9.17.jar
RUN curl -0 https://oss.sonatype.org/content/repositories/google-releases/com/google/appengine/appengine-managed-runtime/1.9.17/appengine-managed-runtime-1.9.17.jar -o /var/lib/jetty/lib/ext/appengine-managed-runtime-1.9.17.jar
ADD . /var/lib/jetty/

RUN ln -s var/lib/jetty/webapps/root /app

WORKDIR /app

# Jetty HTTP port number
EXPOSE 8080
# Java VM debug port number
EXPOSE 5005
# Env variable used by the jetty_run.sh script
ENV RUNTIME_DIR /var/lib/jetty

# Clean any CMD that might be inherited from previous image, because that
# will pollute our ENTRYPOINT, see
# http://docs.docker.io/en/latest/reference/builder/#entrypoint.
CMD []
VOLUME ["/var/log/app_engine"] 
ENTRYPOINT ["/var/lib/jetty/jetty_run.sh"]
