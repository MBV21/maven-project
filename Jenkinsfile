

pipeline {
    agent any
    tools {
        maven 'Local_Maven'
    }
    stages{
        stage('Build') {
            steps {
                // sh 'mvn clean package'
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    // archiveArtifacts artifacts: '**/target/*.war' é 
                    // equivalente a Archive the artifacts, 
                    // Files to archive: **/*.war na interface do Jenkins
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploy to Staging') {
            steps {
                // build job: executa uma tarefa definida no Jenkins
                build job: 'maven-project-deploy-to-staging'
            }
        }
        stage('Deploy to Production') {
            steps {
                timeout(time: 5, unit:'DAYS'){
                    input message:'Approve Production Deployment?'
                }
                build job: 'maven-project-deploy-to-production'
            }
            post {
                sucess {
                    echo 'Code deployed to Production.'
                }
                failure {
                    echo 'Deployment failed.'
                }
            }
        }
    }
}

