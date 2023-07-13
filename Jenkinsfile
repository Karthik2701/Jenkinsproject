pipeline {
    agent { label 'jenkins-master' }
    tools {
        maven 'Maven'
    }

    stages {
        stage('checkout') {
            steps {
               git branch: 'develop', credentialsId: 'git-cred', url: 'https://github.com/Karthik2701/Jenkinsproject.git' 
            }
        }

        stage('build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('publish junit reports') {
            steps {
                junit '**/target/surefire-reports/*.xml'
            }
        }

        stage('SonarQube') {
            environment {
                scannerHome = tool 'SonarQube'
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            //    timeout(time: 10, unit: 'MINUTES') {
              //      waitForQualityGate abortPipeline: true
                //}
            }
        }
	
	stage('Artifact Uploader') {
            steps {
		nexusPublisher nexusInstanceId: '12345', nexusRepositoryId: 'maven-releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: '/var/lib/jenkins/workspace/samplepipeline/target/helloworld.war']], mavenCoordinate: [artifactId: 'hello-world-servlet-example', groupId: 'com.geekcap.vmturbo', packaging: 'war', version: '$BUILD_NUMBER']]]
            }
        }

    }
}
