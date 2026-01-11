pipeline {
    agent any
    
    tools {
        maven 'Maven 3.9.12'
        jdk 'jdk17'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'dev', 
                    url: 'https://github.com/DeBdii/Projet-DevOps-MEZGUELDIMehdi.git'
                echo ' Code récupéré depuis GitHub'
            }
        }
        
        stage('Build') {
            steps {
                dir('demo') {
                    script {
                        try {
                            sh 'mvn clean package'
                            echo ' Build réussi'
                        } catch (Exception e) {
                            echo ' Build échoué: ' + e.toString()
                            currentBuild.result = 'FAILURE'
                            error('Build failed')
                        }
                    }
                }
            }
        }
        
        stage('Test') {
            steps {
                dir('demo') {
                    script {
                        try {
                            sh 'mvn test'
                            echo '✅ Tests passés'
                        } catch (Exception e) {
                            echo ' Tests échoués mais on continue'
                        }
                    }
                }
            }
        }
        
        stage('Archive') {
            steps {
                dir('demo') {
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                    echo 'Artifacts archivés'
                }
            }
        }
        
        stage('Deploy') {
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                echo ' Déploiement simulé'
            }
        }
        
        stage('Notify Slack') {
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                echo ' Notification Slack simulée'
            }
        }
    }
    
    post {
        always {
            echo "📊 Pipeline ${currentBuild.result} - Build #${env.BUILD_NUMBER}"
            cleanWs()
        }
        success {
            echo ' Pipeline réussi!'
        }
        failure {
            echo ' Pipeline échoué!'
        }
    }
}