pipeline {
    agent any
    stages {
	    stage('Clone'){
	        steps{

			bat("""
		        git clone https://github.com/ViktorS01/spring-petclinic
		        """)
	        }
	    }

        stage('Build'){
            steps{
                bat('C:\\build.bat')
            }
	    }

        stage('Archive'){
            steps{
        	    dir('C:\\'){
        			echo "Current build: ${BUILD_NUMBER}"
        			zip zipFile: "${BUILD_NUMBER}.zip", archive:false, dir: 'Windows\\System32\\config\\systemprofile\\AppData\\Local\\Jenkins\\.jenkins\\workspace\\spring-petclinic'
        			archiveArtifacts artifacts: "${BUILD_NUMBER}.zip"
        		}
        	}
        }

        stage('Deploy'){
        		  steps{
        			  dir('C:\\'){
        				  script{
        					  try
        					  {
        						  bat("md C:\\Deploy\\")
        					  }catch(Exception e){}
        				  }
        				  unzip zipFile: "${BUILD_NUMBER}.zip", dir: 'C:\\Deploy'
        			  }
        		  }
        }
    }

    post {
	    always{
		    emailext attachLog: true, body: '''$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS:
		    Check console output at $BUILD_URL to view the results. ${JELLY_SCRIPT, template="html"}''', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'jenkinslaba@gmail.com'
	    }
        cleanup {
            cleanWs()
        }
    }
}
