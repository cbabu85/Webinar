pipeline {
     agent any
     tools { 
      maven 'M2_HOME' 
      jdk 'JAVA_HOME' 
    }
     stages {
       stage('Pullcode') {
         steps {
             git 'https://github.com/cbabu85/Webinar.git'
         }
       }
       stage('Compile') {
         steps {
             sh 'mvn compile'
         }
       }
       stage('Testing') {
          steps {
            sh 'mvn clean test'
            junit 'target/surefire-reports/*.xml'
          }
        }
        //stage('Code coverage') {
        //  steps {
        //     sh 'mvn cobertura:cobertura'
        //     cobertura autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: '**/target/site/cobertura/coverage.xml', failUnhealthy: false, failUnstable: false
       //   }
       // }
        stage('Package') {
          steps { 
            sh 'mvn package -DskipTests=true'
          }
        }
        stage('Deploy to Tomcat') {
          steps {
               input 'Do you approve the deployment..?'
               deploy adapters: [tomcat8(credentialsId: 'deployer_user', path: '', url: 'http://ec2-3-21-246-188.us-east-2.compute.amazonaws.com:8080/')], contextPath: null, onFailure: false, war: '**/*.war'               
        }
     }
  }
}
