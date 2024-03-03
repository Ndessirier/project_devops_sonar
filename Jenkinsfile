pipeline {
    agent any

    tools {
        maven 'maven-3.5.2'
    }

    triggers {
        pollSCM('*/3 * * * *') 
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Compilation') {
            steps {
                script {
                    sh 'mvn clean compile'
                }
            }
        }

        stage('Tests') {
            steps {
                script {
                    // Exécute les tests
                    sh 'mvn test'
                }
            }
        }

        stage('Javadoc') {
            steps {
                script {
                    // Génère la Javadoc
                    sh 'mvn javadoc:javadoc'
                }
            }
        }
        
        stage('SonarQube Analysis') {
            def mvn = tool 'maven-3.5.2';
            withSonarQubeEnv() {
                sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=sonardev -Dsonar.projectName='sonardev'"
    }
  }

    post {
        success {
            echo 'Le pipeline a réussi!'
        }
        failure {
            echo 'Le pipeline a échoué!'
        }
    }
}
