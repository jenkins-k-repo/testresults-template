pipeline {
    agent any

    environment {
        // You can set environment variables here
        MAVEN_OPTS = "-Dmaven.test.failure.ignore=true"
    }

    tools {
        maven 'Maven 3'  // Define your Maven installation name from Jenkins Global Tool Configuration
    }

    stages {
        stage('Build & Test') {
            steps {
                sh 'mvn clean test'
            }
        }

        stage('Registering build artifact') {
            steps {
                echo 'Registering the metadata'
                echo 'Another echo to make the pipeline a bit more complex'
                def artifactOutput1 = registerBuildArtifactMetadata(
                    name: "Automation test for Register-Artifacts",
                    version: "1.0.0",
                    type: "docker",
                    url: "http://non:1111",
                    digest: "6f637064707039346163663237383938",
                    label: "Artifact"
                )
                echo "Artifact output is: ${artifactOutput1}"
                env.ARTIFACT_ID = artifactOutput1            }
        }

        stage('Registering deployed artifact') {
            steps {
                echo "Artifact ID : ${env.ARTIFACT_ID}"
                registerDeployedArtifactMetadata(
                    id: "${env.ARTIFACT_ID}",
                    targetEnvironment: "production"
                )    
                echo 'Deploying...'
                sleep 2
            }
        }

        stage('Publish Test Results') {
            steps {
                junit 'target/surefire-reports/*.xml'
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
        failure {
            echo 'Build or tests failed!'
        }
    }
}
