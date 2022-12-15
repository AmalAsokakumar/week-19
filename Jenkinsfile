pipeline{
    agent any 
    tools{
        maven "maven"
    }
        stages{
        stage("build & SonarQube analysis"){
            steps{
                withSonarQubeEnv('sonarqube') {
                    sh 'mvn clean package sonar:sonar'
                }
            }
        }
        stage("Quality Gate") {
            steps {
              timeout(time: 5, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
              }
            }
        }
        stage("Maven Packaging"){
          steps{
           sh 'mvn package'
          }
        }
        stage("Build Image") {
            steps{
                sh 'docker build -t maven-artifact:$BUILD_NUMBER .'
            }
        }


        stage("EC-repository Docker"){
            steps{
                echo "this is a test stage"
                echo 'login to ecr'
                sh 'aws ecr-public login --region us-east-1 '
                echo 'logged in to the ecr '
        //         // withEnv(["AWS_ACCESS_KEY_ID='${env.AWS_ACCESS_KEY_ID}'", "AWS_SECRET_ACCESS_KEY='${env.AWS_SECRET_ACCESS_KEY}'", "AWS_DEFAULT_REGION='${env.AWS_DEFAULT_REGION}'"]){ //authentication the aws
                    sh 'docker login -u AWS -p$(aws ecr-public get-login-password --region us-east-2) public.ecr.aws/z2t0b6v5'
                    // sh 'docker build -t devop-demo-ecr:$BUILD_NUMBER .'  // need to move it above step 
                    sh 'docker tag devop-demo-ecr:$BUILD_NUMBER public.ecr.aws/z2t0b6v5/devop-demo-ecr:$BUILD_NUMBER'
                    sh 'docker push public.ecr.aws/z2t0b6v5/devop-demo-ecr:latest'

                } 
            }

// access key id :  AKIAVAZX5MABKZLVT3UO
// access key secret : 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
        // stage("EC-repository Docker"){
        //     steps{
        //         withEnv(["AWS_ACCESS_KEY_ID='${env.AWS_ACCESS_KEY_ID}'", "AWS_SECRET_ACCESS_KEY='${env.AWS_SECRET_ACCESS_KEY}'", "AWS_DEFAULT_REGION='${env.AWS_DEFAULT_REGION}'"]){ //authentication the aws
        //             sh 'docker login -u AWS -p$(aws ecr-public get-login-password --region us-east-1) public.ecr.aws/z2t0b6v5'
        //             // sh 'docker build -t devop-demo-ecr:$BUILD_NUMBER .'  // need to move it above step 
        //             sh 'docker tag devop-demo-ecr:$BUILD_NUMBER public.ecr.aws/z2t0b6v5/devop-demo-ecr:$BUILD_NUMBER'
        //             sh 'docker push public.ecr.aws/z2t0b6v5/devop-demo-ecr:latest'

        //         }
        //         // sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/z2t0b6v5'
        //         // sh 'docker tag devop-demo-ecr:$BUILD_NUMBER public.ecr.aws/z2t0b6v5/devop-demo-ecr:$BUILD_NUMBER'
        //         // sh 'docker push public.ecr.aws/z2t0b6v5/devop-demo-ecr:latest'
        //     } 
        // }
        // stage("EC-repository Helm"){
        //     steps{
        //         sh 'aws ecr get-login-password --region ap-northeast-1 | helm registry login --username AWS --password-stdin 266454083192.dkr.ecr.ap-northeast-1.amazonaws.com'
        //         sh "sed -i ' s/tag/'$BUILD_NUMBER'/ ' ./HELM-CHART/values.yaml "
        //         sh "cat ./HELM-CHART/values.yaml "
                    // sh "helm package HELM-CHART "
                    // sh "helm push helm-repo-0.1.0.tgz oci://266454083192.dkr.ecr.ap-northeast-1.amazonaws.com "
        //     }
        // }
        // stage ('TriggerSecondJob') {
        //     steps {
        //         build job: 'pipelineA', parameters: [
        //         string(name: 'param1', value: "value1")
        //         ]
        //     }
        // }
        
    }
}