FROM logstash:latest 

RUN sed 's/^logstash:x:999:/logstash:x:997:/g' -i '/etc/group' \
  && sed 's/^logstash:x:999:999:/logstash:x:997:997:/g' -i '/etc/passwd' \
  && find /usr /var /tmp -user 999 -exec chown 997 {} \; \
  && find /usr /var /tmp -group 999 -exec chown 997 {} \;


