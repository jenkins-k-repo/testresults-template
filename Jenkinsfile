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
        stage('Pre-Registration Wait') {
            steps {
                echo 'Waiting 30 seconds before registering build artifact...'
                sleep(time: 30, unit: 'SECONDS')
                echo 'Wait complete, proceeding to artifact registration'
            }
        }
       stage('Registering build artifact') {
            steps {
                echo 'Registering the metadata'
                echo 'Another echo to make the pipeline a bit more complex'
                registerBuildArtifactMetadata(
                    name: "Automation test for Register-Artifacts",
                    version: "1.0.0",
                    type: "docker",
                    url: "http://non:1111",
                    digest: "6f637064707039346163663237383938",
                    label: "Artifact"
                )
            }
        }
        
        stage('Build & Test') {
            steps {
                sh 'mvn clean test'
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
  
 
