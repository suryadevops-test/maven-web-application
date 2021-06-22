node{
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
	def mavenHome = tool name: "maven 3.8.1"
	stage('CheckoutCode'){
	git credentialsId: '317cd0a9-4fc8-4348-9993-6635b939b415', url: 'https://github.com/suryadevops-test/maven-web-application.git'
	}
	stage('Build'){
	sh "${mavenHome}/bin/mvn clean package"
	
	}
	stage('SonarQubeReport'){
	sh "${mavenHome}/bin/mvn sonar:sonar"
	}
	stage('nexus'){
	sh "${mavenHome}/bin/mvn deploy"
	}
	
	stage('deploy into tomcat'){
	sshagent(['055a1938-20fc-492e-8ff5-3813e6f3bcad']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@15.207.248.80:/opt/apache-tomcat-9.0.46/webapps/"
}
	}
	stage('sending email'){
	emailext body: 'build over', subject: 'build status', to: 'suryasekhar.m@gmail.com'
	}


}
