pipeline{
    tools{
        maven 'M3'
    }
    agent any
    parameters { string(name: 'Test Suite', defaultValue: 'ALL', description: '') }
    matrix {
        axes {
            axis {
                name 'PLATFORM'
                values 'linux', 'mac', 'windows'
            }
        }
    stages{
        stage('Preparation') {
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/SanthanaKrishnanm/jenkins_pipeline.git']]])   
            }
         }
        stage('Build'){
            steps{
                script{
                    if (isUnix()) {
                        sh "'mvn' -Dmaven.test.failure.ignore clean package"
                    } else {
                        bat(/"mvn" -Dmaven.test.failure.ignore clean package/)
                    }
                }
            }
        }
        stage('Pre Deployment Test'){
            steps{
                echo "Unit Testing" 
            }
        }
        stage('UAT-Deploy'){
            steps{
                 sh 'bin/makeindex'
            }
        }
        stage('UAT - Post Deployment Test'){
            steps{
                parallel(
                    FireFox:{
                        echo "Iam Firefox browser"  
                    },
                    Safari: {
                        echo "Iam Safari Browser"
                    },
                    Chrome: {
                        echo "Iam Chrome Browser"
                    },
                    IE: {
                        echo "Iam IE Browser"
                    }
                )
            }
            
        }
    }
        stage('Sanity check - PRD') {
            steps {
                input "Does the UAT environment look ok?"
            }
        }
        stage('PRD Deploy') {
            steps {
                echo 'Deploying in PRD'
            }
        }
        stage('Results'){
            steps{
                 archiveArtifacts 'index.jsp'
            }
        }
    
    }
    post{
        always{
            echo 'Always Run'
        }
        success {
            echo 'successful'
        }
        failure {
            echo 'failed'
        }
        unstable {
            echo 'unstable'
        }
        changed {
            echo 'Pipeline has changed - from failing to successful'
        }
    }
}
