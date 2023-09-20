

pipeline {
    agent any
    tools {
        maven 'Local_Maven'
    }
    stages{
        stage('Build') {
            steps {
                /* sh 'mvn clean package' */
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploy to Staging') {
            steps {
                /* build job -> executa uma tarefa definida no Jenkins */
                build job: 'maven-project-deploy-to-staging'
            }
        }
    }
}