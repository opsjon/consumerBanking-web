pipeline{
    agent any
    
    options{
        timestamps()
        timeout(time: 30, unit: 'MINUTES')
        skipDefaultCheckout()
    }
    
    stages{
        stage('getCode'){
            environment{
                branch = 'master'
                credentials_id = '60b4c49b-3177-40be-aaa2-5e3fdcfb2f6a'
                project_url = 'https://github.com/opsjon/consumerBanking-web.git'
            }
            steps{
                echo 'getCode...'
                checkout([$class: 'GitSCM', branches: [[name: "*/${branch}"]], extensions: [], userRemoteConfigs: [[credentialsId: "${credentials_id}", url: "${project_url}"]]])
            }
        }
        
        stage('build'){
            steps{
                sh 'mvn clean package'
            }
        }
        
        stage('publish'){
            environment{
                tomcat_credentials_id = '3eca4ecd-69b3-4158-b2c8-b6d4f8c02b50'
                tomcat_url = 'http://192.168.155.100:8081'
            }
            steps{
                deploy adapters: [tomcat8(credentialsId: "${tomcat_credentials_id}", path: '', url: "${tomcat_url}")], contextPath: null, war: 'target/*.war'
            }
        }
    }
}
