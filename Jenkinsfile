pipeline {
    agent any
    
    stages {
        stage("checkout") {
            steps {
                git branch: 'main', url: 'https://github.com/nascorreia/course3-jenkins-gs-spring-petclinic.git'
            }
        }
        
        
        stage("build") {
            steps {
                sh "./mvnw package"
            }
        }
        
        stage("capture") {
            steps {
                archiveArtifacts '**/target/*.jar'
                jacoco()
                junit '**/target/surefire-reports/TEST*.xml'
            }
        }
    }
    
    post {
        always {
            emailext body: "${env.BUILD_URL}\n${currentBuild.absoluteUrl}", 
            to: 'tap_spy@ncorreia.eu',
            recipientProviders: [developers()], 
            subject: "${currentBuild.currentResult}: Job ${env.JOB_NAME} [${env.BUILD_NUMBER}]"
        }
    }
}