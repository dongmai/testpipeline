/* groovylint-disable CompileStatic */
pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID = '2958-9979-8296'
        AWS_DEFAULT_REGION = 'us-east-1'
        CLUSTER_NAME = 'default'
        SERVICE_NAME = 'nodejs-container-service'
        TASK_DEFINITION_NAME = 'first-run-task-definition'
        DESIRED_COUNT = '1'
        IMAGE_REPO_NAME = 'testpipeline'
        IMAGE_TAG = "${env.BUILD_ID}"
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
        registryCredential = 'demo-pipeline'
    }

    stages {
    // Tests
    // stage('Unit Tests') {
    //   steps {
    //     script {
    //       sh 'npm install'
    //       sh 'npm test -- --watchAll=false'
    //     }
    //   }
    // }

    // Building Docker images
    stage('Building image') {
      steps {
        script {
          dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
        }
      }
    }

    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
      steps {
        script {
          // This step should not normally be used in your script. Consult the inline help for details.
          withDockerRegistry(credentialsId: "ecr:us-east-1:${registryCredential}", url: "https://${REPOSITORY_URI}") {
            // some block
            dockerImage.push()
          }
        }
      }
    }

    //     stage('Deploy') {
    //      steps{
    //             withAWS(credentials: registryCredential, region: "${AWS_DEFAULT_REGION}") {
    //                 script {
    //             sh './script.sh'
    //                 }
    //             }
    //         }
    //       }
    }
}

