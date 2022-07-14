pipeline {
    agent any
    tools {
       maven 'M2_HOME'
    }
    environment {
        registry= '016003963452.dkr.ecr.us-east-1.amazonaws.com/dockerdemopipeline' 
        registryCredientials = 'jenkins-ecr-login-credentials'
        dockerimage = ''
    }
    stages {
       stage ('checkout') {
           steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/devopscbabu/Springboot-maven.git']]])
            }
        }
        stage('Build') {
            steps {
                sh 'mvn package'
            }
        }
        stage('Building the Image') {
        steps {
            script {
            dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
    }
    }
        stage('Deploy the image to Amazon ECR') {
            steps {
                script {
                    docker.withRegistry('http://' + registry, 'ecr:us-east-1:' + registryCredientials ) {
                    dockerImage.push()
                    }
                }      
            }          
        }
    }
        post {
        success {
            mail bcc: '', body: 'Pipeline build successfully', cc: '', from: 'cbabu85@gmail.com', replyTo: '', subject: 'The Pipeline success', to: 'cbabu85@gmail.com'
        }
        failure {  
            mail bcc: '', body: 'Pipeline build not success', cc: '', from: 'cbabu85@gmail.com', replyTo: '', subject: 'The Pipeline failed', to: 'cbabu85@gmail.com'
         } 
    }
}
