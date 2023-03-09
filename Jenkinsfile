pipeline {
    agent any

    stages {
        stage('code checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/opstree/spring3hibernate.git'
            }
        }
        stage('quality check') {
            parallel {
                stage ('code stability-analysis') {
                    steps {
                        sh 'mvn checkstyle:checkstyle'
                    }
                }
                stage ('code quality-analysis') {
                    steps {
                        sh 'mvn pmd:pmd'
                    }
                }
                stage ('code coverage') {
                    steps {
                        sh 'mvn cobertura:cobertura'
                    }
                }
                stage ('code quality-test') {
                    steps {
                        sh 'mvn surefire:test'
                    }
                }
            }
        }
        stage ('building artifacts') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage ('publishing artifacts') {
            steps {
            nexusArtifactUploader artifacts: [[artifactId: 'Spring3HibernateApp', classifier: '', file: '/var/lib/jenkins/workspace/Assignment-3/target/Spring3HibernateApp.war', type: 'war']], credentialsId: 'admin-nexus', groupId: 'Spring3HibernateApp', nexusUrl: '18.60.44.130:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'spring3hibernate', version: '4.0.0'
            }
        }
    
    }
    post {
        success {
            slackSend channel: 'jenkins_', message: 'Build is Success'
            emailext body: 'Build is Success', subject: 'Jenkins Console Output', to: 'shaik.zabiulla0@gmail.com'
        }
        failure {
            slackSend channel: 'jenkins_', message: 'Build is Failed'
            emailext body: 'Build is Failed', subject: 'Jenkins Console Output', to: 'shaik.zabiulla0@gmail.com'
        }
    }
}
