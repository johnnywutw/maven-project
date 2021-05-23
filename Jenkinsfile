pipeline {
    agent any
    tools{
        maven 'local maven'
    }

    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo '开始存档........'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploy to staging'){
            steps{
                build job:'deploy-to-staging'
            }
        }

         stage ('Deploy to Production'){
            steps{
                timeout(time:5, unit:'DAYS'){
                    input message:'是否部署到生产环境?' 
                }

                build job: 'deploy-to-production'
            }
            post {
                success {
                    echo '代码成功部署到生产环境'
                }

                failure {
                    echo ' 部署失败'
                }
            }
        }

    }
}