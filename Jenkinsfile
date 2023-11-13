pipeline {
    agent any
 
    parameters {
        string(name: 'GIT_REPO', defaultValue: 'https://github.com/krishnamsg/boxfuse-sample-java-war-hello.git', description: 'Git Repository URL')
        string(name: 'BRANCH', defaultValue: 'master', description: 'Git Branch')
        string(name: 'TOMCAT_URL', defaultValue: 'http://192.168.56.101:8090', description: 'Tomcat Server URL')
    }
 
    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout([$class: 'GitSCM', branches: [[name: params.BRANCH]], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: params.GIT_REPO]]])
                }
            }
        }
 
        stage('Build') {
            steps {
                script {
                    sh 'mvn clean package'
                }
            }
        }
 
        stage('Deploy to Tomcat') {
            steps {
                script {
                    def warFile = findFiles(glob: 'target/neogym.war')[0]
                    def tomcatManagerUrl = "${params.TOMCAT_URL}/manager/text"
                    def tomcatCredentials = 'deployer:deployer' // Change this to your Tomcat credentials
 
                    sh "curl --upload-file ${warFile} --user ${tomcatCredentials} ${tomcatManagerUrl}/deploy?path=/neogym"
                }
            }
        }
    }
}
