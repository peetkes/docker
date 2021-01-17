FROM centos:centos7

LABEL Peter Kester <peetkes@gmail.com>

# Get any CentOS updates then clear the Docker cache
RUN yum -y update && yum clean all
# Install MarkLogic dependencies
RUN yum -y install glibc.i686 gdb.x86_64 redhat-lsb.x86_64 && yum clean all
# Install the initscripts package so MarkLogic starts ok
RUN yum -y install initscripts && yum clean all
# Set the Path
ENV PATH /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/opt/MarkLogic/mlcmd/bin
# Copy the MarkLogic installer to a temp directory in the Docker image being built
COPY MarkLogic-*.rpm /tmp/MarkLogic.rpm
# Install MarkLogic then delete the .RPM file if the install succeeded
RUN yum -y install /tmp/MarkLogic.rpm && rm /tmp/MarkLogic.rpm

# Copy the MarkLogic installer to a temp directory in the Docker image being built
COPY MarkLogicConverters-*.rpm /tmp/MarkLogicConverters.rpm
# Install MarkLogic then delete the .RPM file if the install succeeded
RUN yum -y install /tmp/MarkLogicConverters.rpm && rm /tmp/MarkLogicConverters.rpm

# copy init script
COPY init-marklogic /usr/local/sbin

# Expose MarkLogic Server ports
# Also expose any ports your own MarkLogic App Servers use such as
# HTTP, REST and XDBC App Servers for your applications
EXPOSE 7997 7998 7999 8000 8001 8002
# Start MarkLogic from init.d script.
# Define default command (which avoids immediate shutdown)
CMD /etc/init.d/MarkLogic start && tail -f /dev/null