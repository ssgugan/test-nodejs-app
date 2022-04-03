pipeline{
    agent any
    environment {
        PATH = "$PATH:/opt/apache-maven-3.8.5/bin"
    }
    stages{
       stage('GetCode'){
            steps{
                git 'https://github.com/selvigugan/hello-world-1.git'
            }
         }        
       stage('Build'){
            steps{
                sh 'mvn clean package'
            }
         }
        stage('SonarQube analysis') {
//    def scannerHome = tool 'SonarScanner 4.0';
        steps{
        withSonarQubeEnv('sonarqube-7.6') { 
        // If you have configured more than one global server connection, you can specify its name
//      sh "${scannerHome}/bin/sonar-scanner"
        sh "mvn sonar:sonar"
    }
        }
        }
         stage("Quality gate") {
            steps {
                waitForQualityGate abortPipeline: true
            }
        }
        stage("deploy") {
            steps {
                deploy adapters: [tomcat8(credentialsId: 'tomcat', path: '', url: 'http://18.216.52.184:8080/')], contextPath: null, war: '**/*.war'
            }
        }
    }
    post {
        always {
            emailext attachLog: true, body: 'project status', subject: 'project status', to: 'massgugan21@gmail.com'
        }
    }
}
