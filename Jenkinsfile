pipeline{
    tools{
        maven 'M3'
    }
    agent any
    parameters { string(name: 'Environment', defaultValue: 'staging', description: '') }
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
        stage('Deploy'){
            steps{
                 sh 'bin/makeindex'
            }
        }
		stage('Post Deployment Test'){
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
		stage('Results'){
            steps{
                 archiveArtifacts 'index.jsp'
            }
        }
    
    }
    post{
        always{
            echo'I wont fail'
        }
    }
}
