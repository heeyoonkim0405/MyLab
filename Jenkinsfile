pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }

    stages {
        // Specify various stage with in stages

        // stage 1. Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage ('Test'){
            steps {
                echo ' testing......'

            }
        }

        // Stage3: Publish the artifacts to Nexus
        stage ('Publish to Nexus'){
            steps {
                nexusArtifactUploader artifacts: [
                    [
                        artifactId: 'VinayDevOpsLab', 
                        classifier: '', 
                        file: 'target/VinayDevOpsLab-0.0.4-SNAPSHOT.war', type: 'war'
                    ]
                ], 
                credentialsId: 'bd2ad194-5092-447f-a294-07b862ad25b9', 
                groupId: 'com.vinaysdevopslab', 
                nexusUrl: '172.20.10.156:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'VinaysDevOpsLab-SNAPSHOT', 
                version: '0.0.4-SNAPSHOT'
            }
        }

        // Stage4 : Publish the source code to Sonarqube
        stage ('Deploy'){
            steps {
                echo ' deploying......'
            }
        }  
    }
}