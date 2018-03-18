# Base Image
FROM ubuntu:16.04

# Creating the image version
RUN echo "version=1.0.2" > version

# Installing Required Packages
RUN apt-get update
RUN apt-get -yq install vim
RUN apt-get -yq install sudo
RUN apt-get -yq install ssh
RUN apt-get -yq install rsync
RUN apt-get -yq install openjdk-8-jdk
RUN apt-get -yq install openssh-server
RUN mkdir /var/run/sshd

#Changing the root password
RUN echo 'root:quatela12' | chpasswd

# Changing ssd_config files
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

#Enabling environment in Profile 
ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile


# Set the environmental variables as this key for Java 
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64

# ADD Environmental Variables to etc/environment to make it permanent 
RUN echo "JAVA_HOME=$JAVA_HOME" >> /etc/environment
RUN echo "DISPLAY=0" >> /etc/environment

RUN echo "JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64" >> ~./bashrc
RUN echo "DISPLAY=0" >> ~./bashrc
RUN source ~/.bashrc

#Exposing the port 22
EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
