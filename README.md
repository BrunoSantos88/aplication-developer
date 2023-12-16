# Lab Play with docker Node1 Jenkins-Server

-  git clone repository
````
git clone https://github.com/BrunoSantos88/aplication-developer.git
````
- Commandos
````
cd aplication-developer/jenkins-docker
docker-compose up -d
````
- Atraves da porta 8081 acesso ao Jenkin Server
- Acesso chave jenkins
````
docker ps container name
docker exec -ti jenkins cat /var/jenkins_home/secrets/initialAdminPassword
````

# Lab Play with docker Node2  Sonarqube-server

-  git clone repository
````
git clone https://github.com/BrunoSantos88/aplication-developer.git
````
- Commandos
````
cd aplication-developer/sonarqube-docker
docker-compose up -d
````
- Atraves da porta 9001 acesso ao Sonarqube Server

- # Lab Play with docker Node3 Promethues-server

-  git clone repository
````
git clone https://github.com/BrunoSantos88/aplication-developer.git
````
- Commandos
````
cd aplication-developer/promethues-docker
docker-compose up -d
````

- Atraves da porta 9090:Promethues e 3000:grafana acesso ao monitoramento.
