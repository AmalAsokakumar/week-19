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
                echo 'building docker image '
                sh 'docker build -t 18.188.220.54:8083/maven-app:$BUILD_NUMBER .'
                echo 'doker login '
            }
        }
        stage('Pushing the image to Nexus'){
            steps{
                withCredentials([string(credentialsId: 'nexus-password', variable: 'password')]) {
                    sh '''
                        docker login -u admin -p $password 18.188.220.54:8083
                        docker push 18.188.220.54:8083/maven-app:$BUILD_NUMBER
                        docker rmi 18.188.220.54:8083/maven-app:$BUILD_NUMBER
                    '''
                }
            }
        }
        // stage('identifing misconfiguration using datree in helm charts'){
        //     steps{
        //         dir('HELM-CHART') {
        //             sh 'helm datree test .'
        //         }  
        //     }
        // }
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

// sqp_9e212df7e9b2f0b3fa54b77e7be7f7b2cbd0b0f3 sonarscanner java-script 