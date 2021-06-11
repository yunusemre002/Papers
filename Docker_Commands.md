# Portainer  
`docker volume create portainer_data`  
`docker run -d --name=portainer -p 8000:8000 -p 9000:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce` 
 
# Grafana  
`docker run -d --name=grafana -p 3000:3000 -v grafana_data:/var/lib/grafana grafana/grafana`  

# influxdb:
`docker run -d --name=influxdb -p 8086:8086 -v influxdb_data:/var/lib/influxdb2 -v influxdb_conf:/etc/influxdb2 influxdb`
 
```
docker run -d --name=influxdb -p 8086:8086 -v influxdb_data:/var/lib/influxdb2 -v influxdb_conf:/etc/influxdb2 -e DOCKER_INFLUXDB_INIT_MODE=setup -e DOCKER_INFLUXDB_INIT_USERNAME=admin -e DOCKER_INFLUXDB_INIT_PASSWORD=password -e DOCKER_INFLUXDB_INIT_ORG=myOrganizations -e DOCKER_INFLUXDB_INIT_BUCKET=myBucketName -e DOCKER_INFLUXDB_INIT_RETENTION=1w -e DOCKER_INFLUXDB_INIT_ADMIN_TOKEN=u6jYmXFKfgdfgdxcbvxcbcvxbcvb-fbdfgd-fbgdfgdz-nn8vfNq6m influxdb
```





# Change restrt policy
`docker container update --restart always [konteynerName]` 
