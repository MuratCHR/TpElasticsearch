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
```javascript
PUT doc_cours/_mapping
{ 
  "properties": {
    "description": { 
      "type": "text"
     }
   }  
 }
 ```
 
On peut modifier le mapping sans le recréer dans les cas suivants :
- Ajouter de nouveaux champs.
- Modification des paramètres du mapping.

Mais certaines modifications ne sont pas possible sans le recréer :
- Changement du type d'index.
- Suppression de champs existants.

## Global schema
![image](https://github.com/MuratCHR/TpElasticsearch/assets/77275197/fbef91c6-4934-487d-8496-060ac91688f2)

### Comment Elasticsearch stocke ses données et comment certaines de ces notions permettent de gagner en robustesse (en termes de sauvegarde et d’intégrité des données) ?

Elasticsearch stocke ses données en les plaçant dans une structure appelée index. Pour assurer la robustesse des données en termes de sauvegarde et d'intégrité, certaines notions clés sont utilisées.
Il permet la création d'un alias pour un index, ce qui permet de l'utiliser comme un index initial. En modifiant l'index utilisé par l'alias, il devient possible de réaliser des opérations de maintenance sur l'index sans interrompre le service. Il utilise un système de cluster composé de plusieurs noeuds qui peuvent interagir entre eux et avec l'index. Chaque noeud représente une instance d'Elasticsearch et possède un ou plusieurs rôles. Ces rôles déterminent les tâches que chaque noeud peut effectuer. Un noeud avec un rôle de maître (gestion du cluster) et un rôle de données (gestion des index/shards) est obligatoire dans le cluster.
Il est recommandé d'avoir au moins trois noeuds maîtres répartis sur trois machines distinctes dans un cluster. Cette configuration renforce la robustesse d'Elasticsearch en termes d'intégrité des données, car même si un noeud tombe en panne, il reste au moins deux autres capables de prendre le relais et de gérer le cluster.
Les noeuds avec le rôle de données sont responsables de la gestion des index et des shards. Un shard représente une partie de l'index qui est stockée sur un noeud. Ensuite, il est possible de créer un réplica de ce shard et de le stocker sur un autre noeud. Cette approche garantit une redondance des données en cas d'incident avec le shard principal, grâce au shard réplica.

## Group project
https://github.com/Bro7Alexis/elasticSearch
