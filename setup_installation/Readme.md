# Chapter 01 [Setup & Installation]
In this section, we are going to setup elasticsearch using docker-compose which is very convenient and easier compared to setting it up from scratch on a local machine. 
The elasticsearch's version is 8.1.2 which can be easily changed to other versions. For more information about different versions take a look at [docker-hub-elasticsearch-main-page](https://hub.docker.com/_/elasticsearch?tab=tags)

## Start a docker based node:
```commandline
sudo docker-compose up
```

## Check elasticsearch
```commandline
curl localhost:9200
```

## Check kibana
Browse to localhost:5601

## Elastic Safe Architecture
<img src='https://raw.githubusercontent.com/pooya-mohammadi/elasticsearch-notes/master/images/elastic-safe-architecture.png'>