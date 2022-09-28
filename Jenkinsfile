pipeline{
    agent any
    tools { maven "maven 3.8.6"
    }
    stages {
    stage('1Getcode') { 
    steps {
    sh "echo 'cloning the latest application version' "
    git 'https://github.com/lmt2022/maven-web-application'
    }
        }
        stage ('2test&build'){
            steps {
        sh "echo 'running JUnit test cases' " 
            sh "echo 'testing must pass to create artifacts' "
    sh "mvn clean package"    }
    } 
    stage ('3codequality') {
        steps { sh "echo 'perform code quality analysis' "
        sh "mvn sonar:sonar"
        }
    }
    stage ('4upload2nexus') {
        steps { 
            sh "mvn deploy"
            }
    }
stage ('5deploytoprod') {
        steps {
            deploy adapters: [tomcat9(credentialsId: 'admin', path: '', url: 'http://3.91.208.49:8080/')], contextPath: null, war: 'target/*war'
        }
}
    }
    post {
        always {
            emailext body: 'the deployment was a success', recipientProviders: [developers()], subject: 'deployment'
        }
    }
}
