FROM docker.elastic.co/beats/filebeat:8.7.0
COPY filebeat.directory.yml /usr/share/filebeat/filebeat.yml
USER root
RUN mkdir -p /usr/share/filebeat/mylogs
RUN chown root:filebeat /usr/share/filebeat/filebeat.yml
RUN chmod go-w /usr/share/filebeat/filebeat.yml