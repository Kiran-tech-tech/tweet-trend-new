pipeline {
    agent {label 'maven'}


environment {
    PATH = "/opt/apache-maven-3.9.9/bin:$PATH"
    }
    stages {

       stage("build"){
        steps {
            echo "------- build started --------"
            sh 'mvn clean deploy'
            echo "---------build completed ----------"
        }
       
    }
    
    }
}
