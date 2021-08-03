pipeline {
  agent any
  stages {
    stage('Logging into AWS ECR') {
      steps {
        sh 'aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com'
      }
    }

    stage('Cloning git repo') {
      steps {
        sh 'checkout([$class: \'GitSCM\', branches: [[name: \'*/master\']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: \'\', url: \'https://github.com/fopingn/cicd-demo.git\']]])'
      }
    }

    stage('Building image') {
      steps {
        sh ' dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"'
      }
    }

    stage('Pushing to ECR') {
      steps {
        sh '''sh "docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:$IMAGE_TAG"
                sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"'''
      }
    }

  }
  environment {
    AWS_ACCOUNT_ID = '118698220025'
    AWS_DEFAULT_REGION = 'eu-west-3'
    IMAGE_REPO_NAME = 'careveo_prod'
    IMAGE_TAG = 'v_${BUILD_NUMBER}'
    REPOSITORY_URI = '"${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"'
  }
}