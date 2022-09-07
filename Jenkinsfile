@Library(['rig_modules'])_
pipeline {
    agent any
    stages {
        stage('git checkout') {
            steps {
            sh "echo git checkout"
            }
        }
      	stage('maven build') {
        	steps {
        		sh 'mvn clean package'
        	}
        }
      	stage('Update dependency to rig') {
            steps {
                script {
        		    dependencyModule.push("DNHU71DS")
                }
        	}
        }
        stage('sonar analysis') {
            environment {
                scannerHome = tool 'SonarQube'
            }
            steps{
                withSonarQubeEnv('rigSonarQubeServer') {
                    sh '${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=DNHU71DS -Dsonar.exclusions=src/test/java/**'
                }
            }
        }
        stage('maven publish to nexus') {
            steps{
                nexusArtifactUploader artifacts: [[artifactId: 'TomcatMavenApp', classifier: '', file: 'target/TomcatMavenApp-2.0.war', type: 'war']], credentialsId: 'my_nexus', groupId: 'com.sarav', nexusUrl: 'nexus-app.wiprodigitalrig.com/', nexusVersion: 'nexus3', protocol: 'https', repository: 'TomcatMavenApp/', version: '2.0'
            }
        }
    }
}
