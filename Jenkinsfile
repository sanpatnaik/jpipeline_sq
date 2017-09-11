node{

    checkout scm
    //def mvnHome
    dir('BuildQuality'){
        stage('Preparation'){
                        
            git 'https://github.com/sanpatnaik/simple-spring.git'
            //mvnHome = tool 'Maven'
        }
        stage('SonarQube Analysis') { 
           // def mvnHome
            //mvnHome = tool 'Maven'
            def scannerHome = tool 'SonarQube Scanner 2.6.1';
            withSonarQubeEnv('Sonar') { 
                if (isUnix()) {
                    sh "${scannerHome}/bin/sonar-scanner" + 
                    " -Dsonar.projectKey=org.sonarqube:java-sonar_MVN_SQ " +
                    " -Dsonar.projectName='Java :: Simple Spring Project_MVN_SQ' " +
                    " -Dsonar.projectVersion=1.0 " +
                    " -Dsonar.language=java " +
                    " -Dsonar.sources=. " +
                    " -Dsonar.tests=. " +
                    " -Dsonar.test.inclusions='**/*Test*/**' " +
                    " -Dsonar.exclusions='**/*Test*/**' "
                } else {
                    bat (/"${mvnHome}\bin\mvn" org.sonarsource.scanner.maven:sonar-maven-plugin:3.3.0.603:sonar -f pom.xml -Dsonar.projectKey=org.sonarqube:java-sonar_MVN_SQ -Dsonar.projectName="Java :: Simple Spring Project_MVN_SQ" /)
                }    
            }        
        }

        stage('Unit Test Results') {
            junit '**/target/surefire-reports/TEST-*.xml'
            //archive 'target/*.jar'
        }

    }
}

stage name:'Deploy to staging', concurrency:1
    node {
                //sh 'sudo docker run -d -p=3000:80 --network=bundlev2_prodnetwork nginx'
        dir('BuildQuality'){
        sh 'sudo docker-compose up -d --build'
    }
                
}

stage name:'Shutdown staging'
    node {
                
        dir('BuildQuality'){
        sh 'sudo docker-compose stop'
    }
                
}
