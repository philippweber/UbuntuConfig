# ELK on Docker

```bash
# Get Elastic up and running
docker network create es-net

docker run -d --name es-i1 --network es-net -p '127.0.0.1:9201:9200' -v '/var/lib/elasticsearch/instance_1:/usr/share/elasticsearch/data' elasticsearch
docker run -d --name es-i2 --network es-net -p '127.0.0.1:9202:9200' -v '/var/lib/elasticsearch/instance_2:/usr/share/elasticsearch/data' elasticsearch
docker run -d --name es-i3 --network es-net -p '127.0.0.1:9203:9200' -v '/var/lib/elasticsearch/instance_3:/usr/share/elasticsearch/data' elasticsearch

#'-Des.discovery.zen.ping.multicast.enabled=false'
#'-Des.discovery.zen.ping.unicast.hosts=es-i1,es-i2,es-i3'h -f /co
#'-Des.network.host=0.0.0.0'

docker run -d --name es-i1 --network es-net -p '127.0.0.1:9201:9200' -v '/var/lib/elasticsearch/instance_1:/usr/share/elasticsearch/data' elasticsearch '-Ediscovery.zen.ping.unicast.hosts=es-i1,es-i2,es-i3' '-Enetwork.host=0.0.0.0'
docker run -d --name es-i2 --network es-net -p '127.0.0.1:9202:9200' -v '/var/lib/elasticsearch/instance_2:/usr/share/elasticsearch/data' elasticsearch '-Ediscovery.zen.ping.unicast.hosts=es-i1,es-i2,es-i3' '-Enetwork.host=0.0.0.0'
docker run -d --name es-i3 --network es-net -p '127.0.0.1:9203:9200' -v '/var/lib/elasticsearch/instance_3:/usr/share/elasticsearch/data' elasticsearch '-Ediscovery.zen.ping.unicast.hosts=es-i1,es-i2,es-i3' '-Enetwork.host=0.0.0.0'

# Needed or Elastic won't start
sudo sysctl -w vm.max_map_count=262144

# Testing
curl -sS -XGET 'http:/127.0.0.1:9201/_cluster/settings'|jq '.'
curl -sS -XPUT 'http:/127.0.0.1:9201/_cluster/settings' -d '{ "transient" : { "logger.discovery" : "DEBUG" } }'
curl -sS -XGET 'http:/127.0.0.1:9201/_nodes'|jq '.nodes[].name'
curl -sS -XGET 'http:/127.0.0.1:9201/_nodes'|jq '._nodes'
curl -sS -XGET 'http:/127.0.0.1:9201/_nodes'|jq '.nodes[].settings'

# Logastash
docker run -d --name logstash --network es-net -p '127.0.0.1:9600:9600' -v '/var/lib/logstash:/config-dir:ro' -v '/var/log:/mnt/log:ro' logstash -f '/config-dir/logstash.conf'

docker run --rm -d --name logstash --network es-net -p '127.0.0.1:9600:9600' -v '/var/lib/logstash:/config-dir:ro' -v '/var/log:/mnt/log:ro' -e 'http.host=0.0.0.0:9600' logstash -f '/config-dir/logstash.conf'

docker run -d --name logstash --network es-net -p '127.0.0.1:9600:9600' -v '/var/lib/logstash:/etc/logstash:ro' -v '/var/log:/mnt/log:ro' logstash -f '/etc/logstash/conf.d/logstash.conf'

docker run -d --name logstash --network es-net -p '127.0.0.1:9600:9600' -v '/var/lib/logstash:/etc/logstash:ro' -v '/var/log:/mnt/log:ro' -v '/etc/passwd:/etc/passwd:ro' logstash -f '/etc/logstash/conf.d/logstash.conf'

docker run -d --name logstash --network es-net -p '127.0.0.1:9600:9600' -v '/var/lib/logstash:/etc/logstash:ro' -v '/var/log:/mnt/log:ro' logstash_user_997 -f '/etc/logstash/conf.d/logstash.conf'

docker run -d --name logstash --network es-net -p '127.0.0.1:9600:9600' -v '/var/lib/logstash:/etc/logstash:ro' -v '/var/log:/mnt/log:ro' logstash_adm -f '/etc/logstash/conf.d/logstash.conf'

cd $HOME/Downloads/docker_logstash_user_997
docker build -t logstash_user_997 .

cd $HOME/Downloads/docker_logstash_adm
docker build -t logstash_adm .

# Getting some test input into Elastic (i.e. syslog, etc.)
input {
  file{
    id => "input_file_syslog"
    type => "syslog"
    path => "/mnt/log/syslog"
    start_position => "beginning"
  }
  file{
    id => "input_file_kern.log"
    type => "kern.log"
    path => "/mnt/log/kern.log"
    start_position => "beginning"
  }
  file{
    id => "input_file_auth.log"
    type => "auth.log"
    path => "/mnt/log/auth.log"
    start_position => "beginning"
  }
}

filter {
  if [type] == "syslog" or [type] == "kern.log" or [type] == "auth.log" {
    grok {
      id => "filter_syslog"
      match => { "message" => "%{SYSLOGTIMESTAMP:syslog_timestamp} %{SYSLOGHOST:syslog_hostname} %{DATA:syslog_program}(?:\[%{POSINT:syslog_pid}\])?: %{GREEDYDATA:syslog_message}" }
      add_field => [ "received_at", "%{@timestamp}" ]
      add_field => [ "received_from", "%{host}" ]
    }
    date {
      id => "filter_date"
      match => [ "syslog_timestamp", "MMM  d HH:mm:ss", "MMM dd HH:mm:ss" ]
    }
  }
}

output {
  elasticsearch { hosts => ["es-i1:9200","es-i2:9200","es-i3:9200"] }
  #stdout { codec => rubydebug }
}

# Adding Kibana
docker run -d --name kibana --network es-net -p '127.0.0.1:5601:5601' -e 'ELASTICSEARCH_URL=http://es-i1:9200' kibana
docker run -d --name kibana --network es-net -p '127.0.0.1:5601:5601' --link 'es-i1:elasticsearch' kibana
```

