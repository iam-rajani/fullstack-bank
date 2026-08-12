pipeline {
    agent any
       
    tools{
        nodejs 'node16'
        
    }
    environment{
        SCANNER_HOME=tool 'sonar-scanner'
    }
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/iam-rajani/fullstack-bank.git'
            }
        }
         //stage('OWASP FS SCAN') {
            //steps {
              //  dependencyCheck additionalArguments: '--scan ./app/backend --disableYarnAudit --disableNodeAudit', nvdCredentialsId: 'nvd-api-key', odcInstallation: 'DP'
            //        dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
          //  }
        //}
        stage('TRIVY FS SCAN') {
            steps {
                sh "trivy fs ."
            }
        }
         stage('SONARQUBE ANALYSIS') {
            steps {
                withSonarQubeEnv('Sonarqube') {
                    sh " $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Bank -Dsonar.projectKey=Bank "
                }
            }
        }
        stage('Install dependencies'){
            steps{
                sh "npm install"
            }
        }
        stage('Backend'){
            steps{
                dir('/var/lib/jenkins/workspace/BankApp/app/backend'){
                sh "npm install"
                }
            }
        }
        stage('Frontend'){
            steps{
                dir('/var/lib/jenkins/workspace/BankApp/app/frontend'){
                sh "npm install"
            }
        }
    }
     stage('Deploy bankApp'){
            steps{
              sh "npm run compose:up"
        }
    }
}
}
