pipeline{
    agent any 
    environment{
        DOCKER_TAG= getDockerTag()
    }
    stages{
        stage('scm checkout'){
        steps{
            git branch: 'main', credentialsId: '96dca2d1-6f99-4db0-bb95-46d93d6c2ff4', url: 'https://github.com/meghana4-g/hiring_java.git'
    }
        }
    stage ('mvn package'){
        
        steps{
            sh "mvn clean package"
    }
    }
    stage ('Build docker image'){
        steps{
            sh "docker build . -t meghmayu/goa:${DOCKER_TAG}"
            sh "docker images"
    }
    }
        stage ('Push docker image'){
           steps{
           withCredentials([string(credentialsId: 'docker-pw', variable: 'dockerpush')]) {
           sh "docker login -u meghmayu -p ${dockerpush}"
           sh "docker push meghmayu/goa:${DOCKER_TAG}"
           } 
    }
            
    }
    stage ('RunContainer on Dev Server'){
        steps{
        sh "docker run -d -p 99:8080 meghmayu/goa:${DOCKER_TAG}"
    }
    }
    }
}    
def getDockerTag(){
    def tag = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
