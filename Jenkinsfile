node {
    def mavenHome = tool name: 'Maven-3.9.9', type: 'maven'
    
    stage("git clone"){
        git branch: 'main', credentialsId: 'gitcreds', url: 'https://github.com/RaavanGokul/std-web-app.git'
    }
    stage("Maven-sonar Test"){
            sh "${mavenHome}/bin/mvn clean verify sonar:sonar"
    }
    stage("mavenBuild-Nexus"){
        sh "${mavenHome}/bin/mvn clean deploy"
    }
     stage("Sopping Tomcat"){
        sshagent(['Tomcat_server']) {
        sh """
           echo "Stopping the tomcat server"
           ssh -o StrictHostKeyChecking=no ec2-user@172.31.7.190 sudo systemctl stop tomcat
           sleep 10
           """
        }
    }
    stage("Deploy WAR to tomcat"){
        sshagent(['Tomcat_server']) {
        sh "scp -o StrictHostKeyChecking=no target/student-reg-webapp.war ec2-user@172.31.7.190:/opt/tomcat/webapps/student-reg-webapp.war" 
        }
    }
    stage("Startin Tomcat"){
        sshagent(['Tomcat_server']) {
        sh """
            echo "Stopping the tomcat server"
            ssh -o StrictHostKeyChecking=no ec2-user@172.31.7.190 sudo systemctl start tomcat
            sleep 10
           """
        }
    }
}