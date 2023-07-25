node{
    def mavenHome = tool name: 'maven3.9.3'
    
    stage('1 Clone the code'){
        git branch: 'main', url: 'https://github.com/Jaydaleme/maven-app-pub'
        //git 'https://github.com/Jaydaleme/maven-app-pub'
        //sh 'git clone https://github.com/Jaydaleme/maven-app-pub'
        //git url: 'https://github.com/Jaydaleme/maven-app-pub.git'
    }
    
    stage('2 Test and Build'){
        sh "${mavenHome}/bin/mvn clean package"
    }
    
    stage('3 Code Quality analysis'){
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    
    stage('4 Upload to Nexus'){
        sh "${mavenHome}/bin/mvn deploy"
    }
    
    stage('5 Deploy to UAT Tomcat'){
        deploy adapters: [tomcat9(credentialsId: 'Tomcat-credentials', path: '', url: 'http://172.31.26.61:8085/')], contextPath: null, war: 'target/*war'
    }
    
    stage('6 Approval gate'){
        sh "echo 'Application ready for review...'" 
        timeout(time: 2, unit: 'DAYS') {
            input message: 'Application ready for deployment. Please review and approve' 
        }
        
    }
    
    stage('7 Deploy to Prod Tomcat'){
        deploy adapters: [tomcat9(credentialsId: 'Tomcat-credentials', path: '', url: 'http://172.31.26.61:8085/')], contextPath: null, war: 'target/*war'
    }
    
    stage('8 Email Notification'){
        mail bcc: '', body: 'It\'s a success guys, good job!', cc: '', from: '', replyTo: '', subject: 'Build status', to: 'jaydaleme@gmail.com'
    }
}




