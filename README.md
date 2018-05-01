# jagger-docker 


## docker stack demo
1. modify compose file: *docker-compose-sample.yml* by updating _VIRTUAL_HOST_ value
2. create secrets:
   * metasigner-cert
   * metasigner-key
   * metasigner-pass
3. run: 
```bash
    docker stack deploy -c docker-compose-sample.yml jaggerdemo
