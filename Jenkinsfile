pipeline {
    agent { label 'maven' }

    environment {
        PATH = "/opt/apache-maven-3.9.9/bin:$PATH"
    }

    stages {
        stage("build") {
            steps {
                echo "------- build started --------"
                sh 'mvn clean deploy'
                echo "---------build completed ----------"
            }
        }

        stage('SonarQube analysis') {
            environment {
                scannerHome = tool 'sonar-scanner' // sonar scanner name should match the one defined in the tools
            }
            steps {
                // Adding SonarQube environment for analysis
                withSonarQubeEnv('sonarqube-server') {
                    sh "${scannerHome}/bin/sonar-scanner" // Communicates with SonarQube server to send the analysis report
                }
            }
        }
    }
}
