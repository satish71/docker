# Baseline image to use
FROM satish71/atlantis_baseos:1.0.2

# Set the environmental variables as this key for Java and Hadoop functioning 

ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64
ENV HADOOP_HOME /opt/hadoop

# ADD Environmental Variables to etc/environment to make it permanent 
#RUN echo "JAVA_HOME=$JAVA_HOME" >> /etc/environment
RUN echo "HADOOP_HOME=$HADOOP_HOME" >> /etc/environment
#RUN echo "DISPLAY=0" >> /etc/environment

RUN echo "HADOOP_HOME=/opt/hadoop" >> ~/.bashrc
#RUN echo "JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64" >> ~/.bashrc
#RUN echo "DISPLAY=0" >> ~/.bashrc
RUN echo "export HADOOP_HOME=/opt/hadoop" >> ~/.bashrc
RUN echo "export PATH=$PATH:$HADOOP_PATH/bin" >> ~/.bashrc
RUN echo "export PATH=$PATH:$HADOOP_HOME/sbin" >> ~/.bashrc
#RUN source ~/.bashrc


# Install required prerequisites for hadoop
#RUN \
#    apt-get update && apt-get install -yq --reinstall build-essential && \
#    apt-get install -yq \
#    ssh \
#    rsync \
#    vim \
#    pdsh\

RUN \
    apt-get update && apt-get install -yq pdsh

# If you have already downloaded the tgz, add this line OR comment it
# Please note, add unzips to content
RUN mkdir /install
ADD hadoop-3.0.0.tar.gz /install

# Download the distribution from Mirror
#RUN wget http://www-us.apache.org/dist/hadoop/common/hadoop-3.0.0/hadoop-3.0.0.tar.gz

# Unzip the contents, uncomment if downloding from internet
#RUN tar -xzf /install/hadoop-3.0.0.tar.gz

# Move the hadoop standard variable 
RUN mv /install/hadoop-3.0.0 $HADOOP_HOME

# Add users to bash profile
RUN for user in hadoop hdfs yarn mapred; do \
         useradd -U -M -d /opt/hadoop/ --shell /bin/bash ${user}; \
    done && \
    for user in root hdfs yarn mapred; do \
         usermod -G hadoop ${user}; \
    done


# Setting JAVA_HOME in hadoop-env file. the parameter will be appended at the last. 
RUN echo "export JAVA_HOME=$JAVA_HOME" >> $HADOOP_HOME/etc/hadoop/hadoop-env.sh

# Copy all xml from local folder core-site.xml,hdfs-site.xmlvi
ADD *xml $HADOOP_HOME/etc/hadoop/

# Create Required Hadop Data Folders
RUN mkdir $HADOOP_HOME/NameNode
RUN mkdir $HADOOP_HOME/DataNode

# Generate ssh key for the hadoop user
#RUN \
#    ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa && \
#    cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys && \
#    chmod 0600 ~/.ssh/authorized_keys

# format the DataNode & NameNode
#RUN $HADOOP_HOME/bin/hdfs namenode -format


# starting the name node 
#RUN $HADOOP_HOME/sbin/hadoop-daemon.sh start namenode;exit 0
# starting the data node 
RUN $HADOOP_HOME/sbin/hadoop-daemon.sh start datanode;exit 0
# starting the secondaryName node 
#RUN $HADOOP_HOME/sbin/hadoop-daemon.sh start secondarynamenode;exit 0
# starting yarn resource manager
#RUN $HADOOP_HOME/sbin/yarn-daemon.sh start resourcemanager;exit 0
# starting yarn node manager
#RUN $HADOOP_HOME/sbin/yarn-daemon.sh start nodemanager;exit 0
# checking the status
#RUN jps;exit 0 

#Adding Ssh config
ADD ssh_config /root/.ssh/config

#Adding a startup script
ADD start-all.sh /


EXPOSE 22 8088 9870 9864 19888 8042 8888
#CMD /usr/sbin/sshd -D
#CMD start-all.sh