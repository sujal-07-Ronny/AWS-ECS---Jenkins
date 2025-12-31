pipeline {
    agent any

    environment {
        AWS_REGION  = "us-east-1"
        ECS_CLUSTER = "LearnJenkins-Cluster"
        ECS_SERVICE = "LearnJenkinsApp-service"
    }

    stages {

        stage('Install AWS CLI') {
            steps {
                sh '''
                if ! command -v aws >/dev/null 2>&1; then
                  apt update && apt install -y awscli
                fi
                '''
            }
        }

       stage('Register Task Definition') {
    steps {
        withCredentials([
            string(credentialsId: 'aws-access-key', variable: 'AWS_ACCESS_KEY_ID'),
            string(credentialsId: 'aws-secret-key', variable: 'AWS_SECRET_ACCESS_KEY')
        ]) {
            dir('aws') {
                sh '''
                aws ecs register-task-definition \
                  --cli-input-json file://task-definition.json \
                  --region $AWS_REGION
                '''
            }
        }
    }
}

        stage('Deploy to ECS Service') {
            steps {
                withCredentials([
                    string(credentialsId: 'aws-access-key', variable: 'AWS_ACCESS_KEY_ID'),
                    string(credentialsId: 'aws-secret-key', variable: 'AWS_SECRET_ACCESS_KEY')
                ]) {
                    sh '''
                    aws ecs update-service \
                      --cluster $ECS_CLUSTER \
                      --service $ECS_SERVICE \
                      --force-new-deployment \
                      --region $AWS_REGION
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "✅ ECS deployment completed successfully"
        }
        failure {
            echo "❌ ECS deployment failed"
        }
    }
}
