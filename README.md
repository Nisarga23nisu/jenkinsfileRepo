# jenkinsfileRepo

Notes:
  
If previous stages is failure it should not execute next stage for that you can add 
when {
    expression { return currentBuild.current.Result == 'SUCCUESS' }
    }

The when directive checks the result of Build1. The condition currentBuild.currentResult == 'SUCCESS' ensures that the Build12 stage will only run if the previous stage (Build1) completed successfully.


pipeline {
    agent none  
    stages {
        stage('Build1') {
            agent { label 'slave-1' }
            steps {
                sh '''
                    ls -ll
                    echo "This is stage of building1"
                '''
            }
        }
        stage('Build12') {
            // Run Build12 only if Build1 was successful
            when {
                expression { return currentBuild.currentResult == 'SUCCESS' }
            }
            agent { label 'master' }
            steps {
                sh '''
                    ls -ll
                    ls -a
                    echo "This is stage of building2"
                '''
            }
        }
    }
}
