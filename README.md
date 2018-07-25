<h3>Dockerfile for keycloak server with external database (mariaDB)</h3>  
- Install docker on VM and run it as a service   
  
`sudo systemctl enable docker`  
`sudo systemctl start docker`  
  
- Replace database url, login and password in standalone.xml  
  
- build docker image:  
`docker build -t keycloak .`  
- start container:  
`docker run -p 8080:8080 -d --name keycloak --restart unless-stopped keycloak`    
- show logs:  
`docker logs --tail=10 -f container_name`  
  