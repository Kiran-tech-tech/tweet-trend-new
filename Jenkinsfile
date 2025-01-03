def registry = 'https://trialex0wkm.jfrog.io'

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

        stage("Jar Publish") {
            steps {
                script {
                    echo '<--------------- Jar Publish Started --------------->'
                    
                    // Configure Artifactory server
                    def server = Artifactory.newServer url: registry + "/artifactory", credentialsId: "artifact-cred"
                    
                    // Set properties for the uploaded artifacts
                    def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}"
                    
                    // Upload specification
                    def uploadSpec = """{
                        "files": [
                            {
                                "pattern": "jarstaging/(*)",
                                "target": "sunny-libs-release-local/{1}",
                                "flat": "false",
                                "props": "${properties}",
                                "exclusions": ["*.sha1", "*.md5"]
                                
                            }
                        ]
                    }"""
                    
                    // Upload artifacts and publish build info
                    def buildInfo = server.upload(uploadSpec)
                    buildInfo.env.collect()
                    server.publishBuildInfo(buildInfo)
                    
                    echo '<--------------- Jar Publish Ended --------------->'
                }
            }
        }
    }
}

