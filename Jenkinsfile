pipeline{
    agent any 
    tools{
        maven "maven"
    }
        stages{
    //     stage("build & SonarQube analysis"){
    //         steps{
    //             withSonarQubeEnv('sonarqube') {
    //                 sh 'mvn clean package sonar:sonar'
    //             }
    //         }
    //     }
    //     stage("Quality Gate") {
    //         steps {
    //           timeout(time: 1, unit: 'HOURS') {
    //             waitForQualityGate abortPipeline: true
    //           }
    //         }
    //     }
        stage("Maven Packaging"){
          steps{
           sh 'mvn package'
          }
        }
        stage("Build Image") {
            steps{
                sh 'docker build -t devop-demo-ecr:$BUILD_NUMBER .'
            }
        }
        stage("EC-repository Docker"){
            steps{
                sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/z2t0b6v5'
                sh 'docker tag devop-demo-ecr:$BUILD_NUMBER public.ecr.aws/z2t0b6v5/devop-demo-ecr:$BUILD_NUMBER'
                sh 'docker push 266454083192.dkr.ecr.ap-northeast-1.amazonaws.com/maven-app:$BUILD_NUMBER'
            } 
        }
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