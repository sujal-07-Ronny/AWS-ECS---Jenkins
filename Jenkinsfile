pipeline {
    agent any

    environment {
        AWS_REGION  = "us-east-1"
        ECS_CLUSTER = "LearnJenkins-Cluster"
        ECS_SERVICE = "LearnJenkinsApp-service"
    }

    stages {

        stage('Deploy to ECS Fargate') {
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
            echo "✅ ECS deployment triggered successfully (us-east-1)"
        }
        failure {
            echo "❌ ECS deployment failed"
        }
    }
}
