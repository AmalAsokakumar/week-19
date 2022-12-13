pipeline{
    agent any
    tools{
        maven "maven"
    }
    stages{
        stage('build'){
            steps{
                sh 'hello world'

                sh"printenv"
            }
        }
        stage('Publish ECR'){
            steps{
                withEnv(["AWS_ACCESS_KEY_ID=$(env.AWS_ACCESS_KEY_ID)", "AWS_SECRET_ACCESS_KEY=$(env.AWS_SECRET_ACCESS_KEY)", "AWS_DEFAULT_REGION=$(env.AWS_DEFAULT_REGION)"])
                sh 'docker login -u AWS -p $(aws ecr-public get-login-password --region us-east-1) public.ecr.aws/z2t0b6v5'
                sh 'docker build -t devop-demo-ecr .''  
                sh 'docker tag devop-demo-ecr:latest public.ecr.aws/z2t0b6v5/devop-demo-ecr:""$BUILD_ID""'
                sh 'docker push public.ecr.aws/z2t0b6v5/devop-demo-ecr:""$BUILD_ID""'
            }
        }
    }
}