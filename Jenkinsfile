pipeline {   
    
    agent any
    
    stages {
        stage("checkout") {
            steps {
                git branch:'main', url:'https://github.com/drebnija/petclinic'
            }
        }
        
        stage("build") {
            steps {
                bat 'mvnw.cmd package'
            }
        }
        
        stage("capture") {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar'
                junit '**/target/surefire-reports/TEST*.xml'
                jacoco() 
            }
        }
    }
    
    post {
        always {
            emailext body: "${env.BUILD_URL}\n${currentBuild.absoluteUrl}",
                to: 'report@jenkins-p.com',
                recipientProviders: [previous()], 
                subject: "${currentBuild.currentResult}: Job ${env.JOB_NAME} [${env.BUILD_NUMBER}]"
        }
    }
}
