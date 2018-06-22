pipeline{
    agent any
    stages{
        stage('Build && SonarQube analysis'){
            steps{
                sh 'mvn clean package sonar:sonar -Dsonar.projectKey=my-proj -Dsonar.projectName=test-job -Dsonar.projectVersion=1.0 -Dsonar.sources=. -Dsonar.login=8e1edff9c581e4738076ef95731a38abf2b640e7 -Dsonar.host.url=http://34.229.122.119:9000'
            }
                    
            post {
                success {
                    echo 'Now archiving....'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('Deploy to Staging'){
            steps {
                build job: 'deploy-to-staging'
            }
        }

        stage('Deploy to Production'){
            steps {
                timeout(time:5, unit: 'DAYS'){
                    input message:'Approve PRODUCTION Deployment?'
                }

                build job: 'deploy-to-prod'
            }
         
            post{
                success {
                    echo 'Code deployed to production.'
                }
                failure {
                    echo 'Deployment failed.'
                }
            }

        }
             
    }
}
