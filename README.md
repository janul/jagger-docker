# jagger-docker 


## docker stack demo
1. init swarm
   ```bash
   docker swarm init
   ```
2. add label to node where db service will sit:
   ```bash
   docker node update --label-add jaggerdb=true NODE_ID
   ```
3. modify compose file: *docker-compose-sample.yml* by updating _VIRTUAL_HOST_ value
4. create secrets:
   * metasigner-cert
   * metasigner-key
   * metasigner-pass
5. run: 
   ```bash
    docker stack deploy -c docker-compose-sample.yml jaggerdemo
   ```
4. after the first deployment db is empty so you need to populate tables...
   ```bash 
   docker container ls
   ```
   find container id of jaggerdemo_jagger....
   ```bash
   docker exec -t -i CONTAINER_ID /bin/bash
   ```
   inside docker container run
   ```bash
   cd /opt/Jagger/application
   ./doctrine orm:schema-tool:create
   exit
   ```



### TODO 
* finish steps after the first run
* add support for custom http cert
