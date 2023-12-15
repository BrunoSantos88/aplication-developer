# Lab Play with docker

-  git clone repository
````
git clone https://github.com/BrunoSantos88/aplication-developer.git
````
- Commandos
````
cd aplication-developer
docker-compose up -d
````
- Atraves da porta 8081 acesso ao Jenkin Server
````
chave jenkins
docker ps
docker exec -ti jenkins cat /var/jenkins_home/secrets/initialAdminPassword
````

