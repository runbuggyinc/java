#
# © RunBuggy (sasha@runbuggy.com) 2019. All rights reserved.
# CONFIDENTIAL AND PROPRIETARY INFORMATION OF RunBuggy.
#

#FROM 236900137235.dkr.ecr.us-west-2.amazonaws.com/fabric8/java-amazonlinux2023-openjdk17-jre:2023.2.20231023.2 As production
FROM ghcr.io/graalvm/jdk-community:22.0.0-ol9-20240319 As production

RUN mkdir -p /deployments

# JAVA_APP_DIR is used by run-java.sh for finding the binaries
ENV JAVA_APP_DIR=/deployments \
    JAVA_MAJOR_VERSION=22


# Agent bond including Jolokia and jmx_exporter
ADD agent-bond-opts /opt/run-java-options
RUN mkdir -p /opt/agent-bond \
 && curl https://repo.maven.apache.org/maven2/io/fabric8/agent-bond-agent/1.2.0/agent-bond-agent-1.2.0.jar \
          -o /opt/agent-bond/agent-bond.jar \
 && chmod 444 /opt/agent-bond/agent-bond.jar \
 && chmod 755 /opt/run-java-options
ADD jmx_exporter_config.yml /opt/agent-bond/
EXPOSE 8778 9779

# Add run script as /deployments/run-java.sh and make it executable
COPY run-java.sh /deployments/
RUN chmod 755 /deployments/run-java.sh

CMD [ "/deployments/run-java.sh" ]
