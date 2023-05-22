# TpElasticsearch

Install Elasticsearch with Docker
```script
docker pull docker.elastic.co/elasticsearch/elasticsearch:8.7.1
```

Install Kibana
```script
docker pull docker.elastic.co/kibana/kibana:8.7.1
```

Start Elasticsearch containers 
```script
docker run --name es-node01 --net elastic -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -t docker.elastic.co/elasticsearch/elasticsearch:8.7.1
```

Start Kibana 
```script
docker run --name kib-01 --net elastic -p 5601:5601 docker.elastic.co/kibana/kibana:8.7.1
```

Go to the link and copy paste the enrollment token
