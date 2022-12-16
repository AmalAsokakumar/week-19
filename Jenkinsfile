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
        stage('unit test'){
            steps{
                sh 'mvn test'
            }
        }
        stage('integration test'){
            steps {
                sh 'mvn verify  -DskipUnitTests'
            }
        }
        stage('code analysis with checkstyle'){
            steps{
                sh 'mvn checkstyle:checkstyle'
            }
            post {
                success{
                    echo "Generated Analysis Result"
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