env.DOCKER_REGISTRY = 'ajinnf'
env.DOCKER_IMAGE_NAME = 'frontend'
node('master') {
    stage('HelloWorld') {
      echo 'Hello World!'
    }
    stage('Git Clone from Github') {
        git url: 'https://github.com/AjinNF/p-frontend-bigproject.git' 
    }
    stage('Build Docker Image') {    
        sh "docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER} ."   
    }
    stage('Push Docker Image to Docker Hub') {
        sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"
    }
    stage('Deploy To Kubernetes Cluster') {
        sh'''sed -i "15d" front-end-deployment.yml'''
        sh'''sed -i "14 a \'\\'          image: ajinnf/frontend:${BUILD_NUMBER}" front-end-deployment.yml && sed -i "s/''//"  front-end-deployment.yml'''
        sh "kubectl apply -f  front-end-deployment.yml"
   }
    stage('Remove Docker Image in local') {
        sh "docker rmi $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"   
    }
    stage("Clean Workspace"){
        cleanWs()
    }    
}
