node('master'){
   
   stage('git checkout'){
                  git 'https://github.com/Palanimks/Inglibrary.git'
              }
   stage('java build'){
             sh '/opt/maven/bin/mvn clean verify sonar:sonar -Dsonar.password=admin -Dsonar.login=admin'
         }
   stage("build & SonarQube analysis") {
              withSonarQubeEnv('sonar') {
                 sh '/opt/maven/bin/mvn clean deploy sonar:sonar'
              }
          }
      
      stage("Quality Gate"){
          timeout(time: 60, unit: 'SECONDS') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
          }
      }

   stage('Running java backend application'){
             sh 'export JENKINS_NODE_COOKIE=dontKillMe ;nohup java -Dspring.profiles.active=sit -jar $WORKSPACE/target/*.jar &'
         }
}
