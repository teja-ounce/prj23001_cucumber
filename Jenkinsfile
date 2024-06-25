pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/teja-ounce/prj23001_cucumber.git', credentialsId: 'GitLogin'
            }
        }

        stage('Build') {
            steps {
                dir('C:/Users/Teja-OUNCE/git/repository/prj231001CucumberBDD') {
                    bat 'mvn clean install'
                }
            }
        }

        stage('Test') {
            steps {
                dir('C:/Users/Teja-OUNCE/git/repository/prj231001CucumberBDD') {
                    bat """
                    set WEB_DRIVER_PATH=%EDGE_DRIVER_PATH%
                    mvn test
                    """
                }
            }
        }

        stage('Allure Report') {
            steps {
                dir('C:/Users/Teja-OUNCE/git/repository/prj231001CucumberBDD') {
                    bat 'allure generate target/allure-results --clean'
                }
            }
        }
    }

    post {
        always {
            dir('C:/Users/Teja-OUNCE/git/repository/prj231001CucumberBDD') {
                archiveArtifacts artifacts: 'target/*.jar', allowEmptyArchive: true
                junit 'target/surefire-reports/*.xml'
            }
        }
        failure {
            mail to: 'tej540840@gmail.com',
                 subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
                 body: "Something is wrong with ${env.BUILD_URL}"
        }
    }
}
