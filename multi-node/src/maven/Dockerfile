FROM maven:3.6.3-jdk-11-slim

LABEL Peter Kester <peetkes@gmail.com>

COPY entrypoint.sh /usr/local/bin/entrypoint.sh

#Start application
WORKDIR /usr/src/application
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
CMD ["bash"]