pipeline {
    agent any
	tools{nodejs "nodejs"}
    stages {
        stage('Build') { 
            steps {
                echo 'Building'
                sh 'git pull origin master'
                sh 'npm install'
                emailext attachLog: true,
                body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}",
                recipientProviders: [developers(), requestor()],
                to: 'czeslave@gmail.com',
                subject: "Build result"
                
            }
        }
        stage('Test') { 
            when{
                expression {"SUCCESS".equals(currentBuild.currentResult)}
            }
            steps {
                echo 'Testing'
                sh 'npm run test'
            }
        }
    }

    post {
        failure {
            echo 'Success'
            emailext attachLog: true,
                body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}",
                recipientProviders: [developers(), requestor()],
                to: 'czeslave@gmail.com',
                subject: "Build failed in Jenkins"
        }
        success {
            echo 'Fail'
            emailext attachLog: true,
                body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}",
                recipientProviders: [developers(), requestor()],
                to: 'czeslave@gmail.com',
                subject: "Successful build in Jenkins"
        }
    }
}
