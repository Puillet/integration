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
        stage('Deploy') {​​​​​​​
            steps {​​​​​​​
                //withMaven(mavenSettingsConfig: 'maven-config', globalMavenSettingsConfig: 'global-config') {​​​​​​​
                    sh "mvn  -s C:/Users/pierr/.m2/settings.xml deploy"
                //}​​​​​​​
            }​​​​​​​
        }​​​​​​​
        stage('Release') {​​​​​​​
            when {​​​​​​​ expression {​​​​​​​  params['Perform release ?']}​​​​​​​ }​​​​​​​
            steps {​​​​​​​
                script{​​​​​​​
                    pom = readMavenPom file: 'pom.xml'
                }​​​​​​​
                withCredentials([usernamePassword(credentialsId: 'mjidsaa', passwordVariable: 'PASSWORD_VAR', usernameVariable: 'USERNAME_VAR')]){​​​​​​​
                    //withMaven(mavenSettingsConfig: 'maven-config', globalMavenSettingsConfig: 'global-config') {​​​​​​​
                        sh 'git config --global user.email "you@example.com"'
                        sh 'git config --global user.name "Test"'
                        sh 'git branch release/'+pom.version.replace("-SNAPSHOT","")
                        sh 'git push origin release/'+pom.version.replace("-SNAPSHOT","")
                        sh 'mvn release:prepare -s C:/Users/pierr/.m2/settings.xml -B -Dusername=$USERNAME_VAR -Dpassword=$PASSWORD_VAR'
                        sh 'mvn release:perform -s C:/Users/pierr/.m2/settings.xml -B -Dusername=$USERNAME_VAR -Dpassword=$PASSWORD_VAR'
                    //}​​​​​​​
                }​​​​​​​
            }​​​​​​​
        }​​​​​​​
        stage('Sonar') {​​​​​​​
            steps {​​​​​​​
                withCredentials([usernamePassword(credentialsId: 'mjidsaa', passwordVariable: 'PASSWORD_VAR', usernameVariable: 'USERNAME_VAR')]) {​​​​​​​
                    sh "mvn  -s C:/Users/pierr/.m2/settings.xml sonar:sonar -Dsonar.login=admin -Dsonar.password=admin123"
                }​​​​​​​
            }
        }
    }
}
