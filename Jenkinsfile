

pipeline {
    agent any
    parameters {
        // string(name: 'tomcat_dev', defaultvalue: '10.32.109.55:48091', description: 'Staging server')
        // string(name: 'tomcat_prod', defaultvalue: '10.32.109.55:48092', description: 'Production server')
        string(name: 'tomcat_dev', defaultvalue: 'C:\apache-tomcat-1-staging\webapps')
        string(name: 'tomcat_prod', defaultvalue: 'C:\apache-tomcat-2-production\webapps')
    }
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage('Build') {
            steps {
                // sh 'mvn clean package'
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deployments') {
            parallel {
                stage ('Deploy to Staging') {
                    steps {
                        bat "copy .\target\*.war ${params.tomcat_dev}"
                    }
                }
                stage("Deploy to Production") {
                    steps {
                        bat "copy .\target\*.war ${params.tomcat_prod}"
                    }
                }
            }
        }
    }
}

