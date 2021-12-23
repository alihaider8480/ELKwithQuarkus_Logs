# ELKwithQuarkus_Logs
elk with quarkus and docker ma confgiguration dali h phir usko run kias ha


first installed these three images in our system 

docker pull elasticsearch:7.15.0
docker pull logstash:7.16.2
docker pull kibana:7.16.2

when we done installed these 3 things then we create a docker yml file and there we write these three docker configuration those I wrote down

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

isko run karna is comand sa or jab file save karna to docer-compose sa ni karni ha isko elk.yml sa save karna ha or phir is command sa run karna ha
docker-compose -f elk.yml up

version: '3.5'

services:
  elasticsearch:
    image: elasticsearch:7.16.2
    ports:
      - "9200:9200"
      - "9300:9300"
    environment:
      ES_JAVA_OPTS: "-Xms512m -Xmx512m"
      discovery.type: single-node
    networks:
      - elk
      
  logstash:
    image: logstash:7.16.2
    volumes:
      - source: $HOME/pipelines
        target: /usr/share/logstash/pipeline
        type: bind
    ports:
      - "12201:12201/udp"
      - "5000:5000"
      - "9600:9600"
    networks:
      - elk
    depends_on:
      - elasticsearch
      
  kibana:
    image: kibana:7.16.2
    ports:
      - "5601:5601"
    networks:
      - elk
    depends_on:
      - elasticsearch
    
networks:
  elk:
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
gelf.conf

input {
  gelf {
    port => 12201
  }
}
output {
  stdout {}
  elasticsearch {
    hosts => ["http://elasticsearch:9200"]    // ma yaha pa elasticsearch ki jaga local host bhe likha sakta tha lakin yaha ma ma apna container ka name donga
  }
}
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
ab agr hum java ki application ko hit maronga to wo logstash ma jae ga or log stash sa elasticsearch ma jae ga 
or kibana usko khudi waha pa show kardaga matlab mujha kibana ka ui ma koi configuration ni karni ha.

![Screenshot from 2021-12-22 19-37-26](https://user-images.githubusercontent.com/40827670/147109235-8e5ee900-8599-4389-b0b1-54f60064e541.png)


![Screenshot from 2021-12-22 19-40-35](https://user-images.githubusercontent.com/40827670/147109618-1b88e8a8-4795-48cb-8012-39e04b6176e6.png)

![Screenshot from 2021-12-22 19-41-11](https://user-images.githubusercontent.com/40827670/147109703-5e0f0a8d-fd99-4785-9c59-3e06253dc4d4.png)


![Screenshot from 2021-12-22 19-42-08](https://user-images.githubusercontent.com/40827670/147109941-720adaf5-37ad-47c6-ba28-e97d62121637.png)


![Screenshot from 2021-12-22 19-42-18](https://user-images.githubusercontent.com/40827670/147191092-ebfead7b-1b2d-4b3c-b695-d3f52365529c.png)

![1](https://user-images.githubusercontent.com/40827670/147191357-2a389d35-2637-4ebc-aee5-73bb3525da17.png)

![2](https://user-images.githubusercontent.com/40827670/147191388-3783ec40-c8dd-485b-8a90-155a5ae63469.png)

![3](https://user-images.githubusercontent.com/40827670/147191408-c6f86b60-4596-4f7b-bd99-e364e67e036c.png)

![4](https://user-images.githubusercontent.com/40827670/147191437-aaf1273b-c33f-4283-87c2-7ff5571fb90f.png)
