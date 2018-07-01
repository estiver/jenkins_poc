# jenkins_poc

##Criando a versao inicial com backup
sudo docker build . --tag ehipolito/missao-devops-jenkins:0.2.0

sudo docker run -p 8082:8080 -v jenkins_backup:/srv/backup  ehipolito/missao-devops-jenkins:0.2.0

sudo docker inspect jenkins_docker |grep backup

##Criando novo job
CRUMB_NEW=$(curl -s 'http://admin:admin123@127.0.0.1:8082/crumbIssuer/api/xml?xpath=concat(//crumbRequestField,":",//crumb)')
cat jobs/job1_config.xml |  curl -X POST -H $CRUMB_NEW 'http://admin:admin123@127.0.0.1:8082/createItem?name=job_1' --header "Content-Type: application/xml" -d @-


##atualiza job
cat jobs/job1_config.xml |  curl -X POST -H $CRUMB_NEW 'http://admin:admin123@127.0.0.1:8082/job/job_1/config.xml' --header "Content-Type: application/xml" -d @-
