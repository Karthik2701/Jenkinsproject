@Library("my-shared-library") _
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
	       mvnBuild()
               //sh 'mvn clean install'
            }
        }

        stage('publish junit reports') {
            steps {
                junit()
                //junit '**/target/surefire-reports/*.xml'
            }
        }

        stage('SonarQube') {
            environment {
                scannerHome = tool 'SonarQube'
            }
            steps {
                sonarQube()
                //withSonarQubeEnv('SonarQube') {
                    //sh "${scannerHome}/bin/sonar-scanner"
                }
               // timeout(time: 10, unit: 'MINUTES') {
                 //   waitForQualityGate abortPipeline: true
                //}
            }
        }

        stage('Artifact Uploader') {
            steps {
                nexus()
                //nexusPublisher nexusInstanceId: '12345', nexusRepositoryId: 'maven-releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: '/var/lib/jenkins/workspace/samplepipeline/target/helloworld.war']], mavenCoordinate: [artifactId: 'hello-world-servlet-example', groupId: 'com.geekcap.vmturbo', packaging: 'war', version: '$BUILD_NUMBER']]]
            }
        }

        stage('Deploy war to server') {
            steps {
                tomcat()
                //deploy adapters: [tomcat9(credentialsId: 'tomcat-cred', path: '', url: 'http://3.83.100.83:8080')], contextPath: 'samplepipeline', war: '**/*.war'
            }
        }

    }
}
