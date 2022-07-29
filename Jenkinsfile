pipeline
{
agent {label 'ansible_node_build'}
environment
{
DOCKERHUB_CREDENTIALS = credentials ('docker-hub-login')
}
stages
{
stage('Build maven project')

 {

 steps 

  {
   echo 'building'
    sh 'mvn package -X'
    sh 'mvn install -X'
 
  } 

  }
 stage('Build Docker image using ansible')
{
steps
{
 sh 'ansible-playbook ansible_builddockerimage.yml'
} 

}
 stage('docker hub login')

 {

 steps
 {
   sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'

   
 } 

 }
  stage('push docker image')

 {

 steps
 {
   sh 'docker push deepakkumarawsdevops/k83image:1'


 }
}

 stage('Delopyment docker image on k83 cluster')

 {
 agent { label 'k83_node' }

 steps
 {
  sh 'kubectl apply -f k83_deployment.yaml'
  sh 'kubectl apply -f 	k83_service.yaml'
 }
}
  }
} 

