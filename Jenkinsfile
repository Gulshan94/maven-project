pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '13.233.89.32', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '52.66.101.42', description: 'Production Server')
         string(name: 'war_path', defaultValue:"C:/Program Files (x86)/Jenkins/workspace/FullyAutomated/webapp/target/*.war", description: 'full path to war file')
    } 
 
    triggers {
         pollSCM('* * * * *') // Polling Source Control
    }
 
    stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
 
        stage('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        bat "echo y | pscp -i D:/Udemy/DevOps-Jenkins/tomcat-demo.pem ${war_path} ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }
 
                stage ("Deploy to Production"){
                    steps {
                        bat "echo y | pscp -i D:/Udemy/DevOps-Jenkins/tomcat-demo.pem ${war_path} ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}