pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '54.173.163.73', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '54.172.54.17', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /root/.ssh/id_rsa **/target/*.war root@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /root/.ssh/id_rsa **/target/*.war root@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
