pipeline {
    agent any
    tools {
        maven 'maven3.6.3'
        jdk 'jdk9'
    }
    parameters {
        booleanParam(name: "Perform release ?", description: '', defaultValue: false)
    }
    stages {
        stage('Initialize'){
            steps {
                bat '''
                    echo "PATH ${PATH}"
                    echo "M2_HOME ${M2_HOME}"
                ''' 
            }
        }
        stage('Build') {
            steps {
                bat 'mvn compile'
            }
        }
        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }
    }
}
