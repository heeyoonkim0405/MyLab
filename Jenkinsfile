pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }

    environment {
       ArtifactId = readMavenPom().getArtifactId()
       Version = readMavenPom().getVersion()
       Name = readMavenPom().getName()
       GroupId = readMavenPom().getGroupId()
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
                script{
                    
                    def NexusRepo = Version.endsWith("SNAPSHOT") ? "VinaysDevOpsLab-SNAPSHOT" : "VinaysDevOpsLab-RELEASE"
                    
                    nexusArtifactUploader artifacts: 
                    [
                        [
                            artifactId: "${ArtifactId}",
                            classifier: '',
                            file: "target/${ArtifactId}-${Version}.war",
                            type: 'war'
                        ]
                    ],
                    credentialsId: 'bd2ad194-5092-447f-a294-07b862ad25b9',
                    groupId: "${GroupId}",
                    nexusUrl: '172.20.10.156:8081',
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    repository: "${NexusRepo}",
                    version: "${Version}"
                }
            }
        }      

        // Stage4 : Print some information
        stage ('Print Environment variables'){
            steps {
                echo "Artifact ID is '${ArtifactId}'"
                echo "Version is '${Version}'"
                echo "GroupID is '${GroupId}'"
                echo "Name is '${Name}'"
            }
        }
/*
        // Stage5 :
        //  - Login to Ansible Controller over SSH (from Jenkins)
        //  - Run the playbook from Ansible Controller to download the source code to Tomcat server.
        stage ('Deploy to tomcat'){
            steps {
                echo ' deploying......'
                sshPublisher(publishers: 
                [sshPublisherDesc(
                    configName: 'Ansible_Controller', 
                    transfers: 
                    [sshTransfer(
                        cleanRemote: false, 
                        excludes: '', 
                        execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy_as_tomcat_user.yaml -i /opt/playbooks/hosts', 
                        execTimeout: 120000, 
                        flatten: false, 
                        makeEmptyDirs: false, 
                        noDefaultExcludes: false, 
                        patternSeparator: '[, ]+', 
                        remoteDirectory: '', 
                        remoteDirectorySDF: false, 
                        removePrefix: '', sourceFiles: ''
                        )
                    ], 
                    usePromotionTimestamp: false, 
                    useWorkspaceInPromotion: false, 
                    verbose: false)
                ])
            }
        }
*/
        // Stage6 :
        //  - Login to Ansible Controller over SSH (from Jenkins)
        //  - Run the playbook from Ansible Controller to 
        //      - download the latest war file to Docker host
        //      - create Dockerfile,Build Image and run Docker Container
        stage ('Deploy to Docker'){
            steps {
                echo ' deploying......'
                sshPublisher(publishers: 
                [sshPublisherDesc(
                    configName: 'Ansible_Controller', 
                    transfers: 
                    [sshTransfer(
                        cleanRemote: false, 
                        excludes: '', 
                        execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy_docker.yaml -i /opt/playbooks/hosts', 
                        execTimeout: 120000, 
                        flatten: false, 
                        makeEmptyDirs: false, 
                        noDefaultExcludes: false, 
                        patternSeparator: '[, ]+', 
                        remoteDirectory: '', 
                        remoteDirectorySDF: false, 
                        removePrefix: '', sourceFiles: ''
                        )
                    ], 
                    usePromotionTimestamp: false, 
                    useWorkspaceInPromotion: false, 
                    verbose: false)
                ])
            }
        }
    }
}