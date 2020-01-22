pipeline {
    agent any

    tools{
        maven 'local maven'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }

            post {
                success{
                    echo 'start saving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }


        stage('Deploy to staging'){
            steps {
                build job: 'deploy-to-staging'
            }        
        }

        stage('Deploy to Production'){
            steps{
                timeout(time:5, unit:'DAYS'){
                    input message:'是否?'
                }

                build job: 'deploy-to-production'
            }

            post {
                success{
                    echo 'suceess!'
                }

                failure{
                    echo 'failed!'
                }

            }    

        }

    }
}
