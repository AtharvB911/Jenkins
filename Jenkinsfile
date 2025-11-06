pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo '----- Build Stage Started -----'
                echo 'Compiling or preparing application files...'

                bat '''
                @echo off
                echo Building on %COMPUTERNAME%
                if exist out rmdir /s /q out
                mkdir out
                echo Simulated build complete > out\\build_log.txt
                '''
                echo '----- Build Stage Completed -----'
            }
        }

        stage('Test') {
            steps {
                echo '----- Test Stage Started -----'
                bat '''
                @echo off
                echo Running unit tests...
                echo All tests passed successfully > out\\test_results.txt
                '''
                echo '----- Test Stage Completed -----'
            }
        }

        stage('Deploy') {
            steps {
                echo '----- Deploy Stage Started -----'
                bat '''
                @echo off
                echo Deploying application to test environment...
                echo Deployment successful > out\\deploy_log.txt
                '''
                echo '----- Deploy Stage Completed -----'
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline executed successfully — Build, Test, and Deploy all passed!'
        }
        failure {
            echo '❌ Pipeline failed — please check the console output for errors.'
        }
    }
}
