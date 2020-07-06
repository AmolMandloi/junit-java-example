pipeline {
    agent any
    stages {
        stage("Git checkout") {
            steps {
                git 'https://github.com/AmolMandloi/junit-java-example.git'
            }
        }
        stage("Maven") {
            steps {
                bat "mvn clean package test"
            }
        }
        stage("build & SonarQube analysis") {
            steps {
    withSonarQubeEnv('sonar') {
                
                bat 'mvn sonar:sonar -Dsonar.junit.reportsPath=target/surefire-reports/TEST-com.javacodegeeks.examples.junitmavenexample.CalculatorTest.xml'
              }    } }
            
       stage("Quality Gate"){
    options {
        timeout(time: 5, unit: 'MINUTES') 
    }
    steps {
        script {
            def qg = waitForQualityGate()
            if (qg.status != 'OK') {
                error "Pipeline aborted due to quality gate failure: ${qg.status}"
        }}
    }
}

    /*stage("Nexus Artifact Uploader") {
        steps {
            nexusArtifactUploader artifacts: [[artifactId: 'spring-boot-web-thymeleaf8', classifier: '', file: 'C:\\Program Files (x86)\\Jenkins\\workspace\\nexus\\target\\spring-boot-web-thymeleaf-1.0.war', type: 'war']], credentialsId: 'nexus', groupId: 'org.springframework.boot', nexusUrl: 'localhost:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'sample-release', version: '1.0'
        }
    }   */
      
        
        stage("Final") {
            steps{
                bat 'echo "All done"'
            }
        }
        
    }
}
