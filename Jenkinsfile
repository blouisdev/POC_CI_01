pipeline {
    agent {
        docker {
            image 'maven:3.8.5-openjdk-11'
            args '-v /root/.m2:/root/.m2'
        }
    }
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
		stage('SonarQube analysis') {
			def scannerHome = tool 'SonarScanner 4.0';
			withSonarQubeEnv(credentialsId: '35905528e393f847949f3c9d1feecfde7a7afc54', installationName: 'local')
			{
				sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
			}
		}
		
    }
}