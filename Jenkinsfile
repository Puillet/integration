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
        stage('Deploy') {
            steps {
                bat "mvn -s C:/Users/pierr/.m2/settings.xml deploy"
            }
        }
        stage('Release') {
            when { expression { params['Perform release ?']} }
            steps {
                script {
                    pom = readMavenPom file: 'pom.xml'
                }
                withCredentials([usernamePassword(credentialsId: 'admin', passwordVariable: 'PASSWORD_VAR', usernameVariable: 'USERNAME_VAR')]){
                    bat 'git config --global user.email "you@example.com"'
                    bat 'git config --global user.name "Test"'
                    bat 'git branch release/'+pom.version.replace("-SNAPSHOT","")
                    bat 'git push origin release/'+pom.version.replace("-SNAPSHOT","")
                    bat 'mvn release:prepare -s C:/Users/pierr/.m2/settings.xml -B -Dusername=$USERNAME_VAR -Dpassword=$PASSWORD_VAR'
                    bat 'mvn release:perform -s C:/Users/pierr/.m2/settings.xml -B -Dusername=$USERNAME_VAR -Dpassword=$PASSWORD_VAR'
                }
            }
        }
        stage('Sonar') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'admin', passwordVariable: 'PASSWORD_VAR', usernameVariable: 'USERNAME_VAR')]){
                    bat "mvn  -s C:/Users/Majid/.m2/settings.xml sonar:sonar -Dsonar.login=admin -Dsonar.password=admin"
                }
            }
        }
    }
}
