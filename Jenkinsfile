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
       stage('Registering build artifact') {
            steps {
                echo 'Registering the metadata'
                echo 'Another echo to make the pipeline a bit more complex'
                registerBuildArtifactMetadata(
                    name: "test-artifact-cherryl",
                    version: "1.0.0",
                    type: "docker",
                    url: "http://non:1111",
                    digest: "6f637064707039346163663237383938",
                    label: "prod"
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
        // stage('Check Test Report Files') {
        //     steps {
        //         sh '''
        //         echo "📂 Listing test report files..."
        //         ls -l target/surefire-reports/*.xml || echo "❌ No XML files found"
        //         '''
        //     }
        // }
        // stage('Publish Test Results') {
        //     steps {
        //         junit testResults: 'target/surefire-reports/*.xml', allowEmptyResults: true, skipMarkingBuildUnstable: false
        //     }
        // }
        
      
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
  
 
