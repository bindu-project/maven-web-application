node {
    def MavenHome = tool name: "Maven 3.8.2"
    stage('CheckOutCode')
    {
        git branch: 'master', credentialsId: '8a275ea2-210c-4c7d-a3e4-34d9a9c605ad', url: 'https://github.com/bindu-project/maven-web-application.git'
    }
    stage('Build')
    {
        sh "${MavenHome}/bin/mvn clean package"
    }
    stage('SonarQubeReport')
    {
        sh "${MavenHome}/bin/mvn clean package sonar:sonar"
    }
    stage('UploadArtifactIntopNexus')
    {
        sh "${MavenHome}/bin/mvn clean deploy"
    }
    stage('DeployApplicationIntoTomcat')
    {
    sshagent(['ec89cf6c-1a82-42ca-8708-3cf5a34a2cff']) {
        sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.234.112.231:/opt/apache-tomcat-9.0.52/webapps/"
    }
    }
    stage('SendEmail'){
    emailext body: '''Build Success..

    Regards
    Bindu''', subject: 'Build Over', to: 'bindumadhavi.naidu9@gmail.com'    
    }
    
}
