# TpElasticsearch

## Installation Elasticsearch et Kibana

### Install Elasticsearch with Docker
```script
docker pull docker.elastic.co/elasticsearch/elasticsearch:8.7.1
```

### Install Kibana
```script
docker pull docker.elastic.co/kibana/kibana:8.7.1
```

### Start Elasticsearch containers 
```script
docker run --name es-node01 --net elastic -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -t docker.elastic.co/elasticsearch/elasticsearch:8.7.1
```

### Start Kibana 
```script
docker run --name kib-01 --net elastic -p 5601:5601 docker.elastic.co/kibana/kibana:8.7.1
```

### Go to the link and copy paste the enrollment token

### Log in
user : elastic

password : generated previously

## Create index with explicit mappings
```javascript
PUT /doc_cours
{
  "mappings": {
    "properties": {
      "filename": { "type": "text" },
      "description": { "type": "text" },
      "filesize": { "type": "float" },
      "datecrea": { "type": "date"}
    }
  }
}
```

## Index documents 
```javascript
PUT /doc_cours/_doc/1
{
  "filename": "Cours 1",
  "filesize": "1",
  "datecrea": "2023-05-22"
}
```

## With Bulk
```javascript
POST /_bulk
{ "index": { "_index":"doc_cours", "_id":"2"} }
{ "filename": "Cours 2", "filesize" : "1", "datecrea": "2023-05-22" }
{ "index": { "_index":"doc_cours", "_id":"3"} }
{ "filename": "Cours 3", "filesize" : "1", "datecrea": "2023-05-22" }
{ "index": { "_index":"doc_cours", "_id":"4"} }
{ "filename": "Cours 4", "filesize" : "1", "datecrea": "2023-05-22" }
{ "index": { "_index":"doc_cours", "_id":"5"} }
{ "filename": "Cours 5", "filesize" : "1", "datecrea": "2023-05-22" }
``` 

## Search documents
```javascript
GET doc_cours/_doc/1
```

## Search with args 
```javascript
GET doc_cours/_search
{
  "query": {
    "match_phrase": {
      "filename": "Cours 1"
      }
  }
}
```

## Mapping modifcation
```javapscript
PUT doc_cours/_mapping
{ 
  "properties": {
    "description": { 
      "type": "text"
     }
   }  
 }
 ```
 
 On peut modifier la mapping sans le recréer dans les cas suivants :
 - Ajouter de nouveaux champs.
 - Modification des paramètres du mapping.

 Mais certaines modifications ne sont pas possible sans recréer l'index :
 - Changement du type d'index.
 - Suppression de champs existants.

