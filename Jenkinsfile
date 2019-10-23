node('master'){
   
   stage('git checkout'){
                  git 'https://github.com/Palanimks/Inglibrary.git'
              }
   stage("SonarQube analysis") {
              withSonarQubeEnv('sonar') {
                 sh '/opt/maven/bin/mvn clean sonar:sonar'
              }
          }
   stage('Build'){
             sh '/opt/maven/bin/mvn clean install'
         }
      
      stage("Quality Gate"){
          timeout(time: 60, unit: 'SECONDS') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
          }
      }
   stage('Deploy'){
             sh '/opt/maven/bin/mvn deploy'
         }

   stage('Running java backend application'){
             sh 'export JENKINS_NODE_COOKIE=dontKillMe ;nohup java -Dspring.profiles.active=sit -jar $WORKSPACE/target/*.jar &'
         }
}
