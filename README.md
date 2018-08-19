# jagger-docker in docker swarm
## services
### jagger web
1. image on docker hub: janul/jagger
2. support environment variables and their default values:
- JAGGER_DOCKER = 1  ```do not override this variable```
- JAGGER_WEB = 1 ```tells if container should run web```
- JAGGER_MAILER = 0 ```run monitor to send mail from the queue.```
- JAGGER_CRON = 0 ``` run tasks scheduler```
- JAGGER_MSIGNER_WRK = 0 ``` run rabbitmq consumer for signing metadata``` 
- MEMCACHE_HOST ```Required:  hostname of memcache server```
- DB_HOST ```Required: hostname of mysql server```
- DB_PASSWORD: ```Required: db password```
- DB_USER: ```Required: db user```
- DB_CHARSET = latin1
- VIRTUAL_HOST ```Required```
- JAGGER_URI = /rr3 ```Alias with webroot of application: Do not add / at the end```


### memcache
Required
### rabbitmq
Optional
### db
you can either use exernal db or create service in the stack
### proxy
Tested successfuly with https://github.com/jwilder/nginx-proxy  and https://traefik.io/ however nginx-proxy  cannot (at least for now) discover containers on remote docker nodes.


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

