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
    If you run it first time (empty db) you need to enable env variable in compose file: JAGGER_SETUP : true . It will activate URI /rr3/setup
    
4. create secrets (https://docs.docker.com/engine/swarm/secrets/):
    * metasigner-cert
    * metasigner-key
    * metasigner-pass
    * https_ca
    * https_crt
    * https_key

5. run: 
   ```bash
    # docker stack deploy -c docker-compose-sample.yml jaggerdemo
   ```
6. after the first deployment db is empty so you need to populate tables...
   ```bash 
   # docker container ls
   ```
   find container id of jaggerdemo_jagger....
   ```bash
   # docker exec -t -i CONTAINER_ID /bin/bash
   ```
   inside docker container run
   ```bash
   # cd /opt/Jagger/application 
   ```
   ```bash
   #./doctrine orm:schema-tool:create
   ```




### TODO 
* simplify populating tables
* add support to run jagger more than one replica
* add option to add override config 
* mailing
* tasks scheduler
