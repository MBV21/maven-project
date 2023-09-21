

pipeline {
    agent any
    parameters {
        // string(name: 'tomcat_dev', defaultvalue: '10.32.109.55:48091', description: 'Staging server')
        // string(name: 'tomcat_prod', defaultvalue: '10.32.109.55:48092', description: 'Production server')

        string(name: 'artifact', defaultValue: '.\\webapp\\target\\*.war')
        string(name: 'tomcat_dev', defaultValue: '"C:\\apache-tomcat-1-staging\\webapps"')
        string(name: 'tomcat_prod', defaultValue: '"C:\\apache-tomcat-2-production\\webapps"')
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
                        // sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                        // bat 'copy /y .\\webapp\\target\\webapp.war C:\\apache-tomcat-1-staging\\webapps\\webapp.war'
                        bat 'copy /y ${params.artifact} ${params.tomcat_dev}'
                    }
                }
                stage("Deploy to Production") {
                    steps {
                        // sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                        // bat 'copy /y .\\webapp\\target\\webapp.war C:\\apache-tomcat-2-production\\webapps\\webapp.war'
                        bat 'copy /y ${params.artifact} ${params.tomcat_prod}'
                    }
                }
            }
        }
    }
}

