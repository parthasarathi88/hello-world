pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'network-share-creds',
                    usernameVariable: 'USERNAME',
                    passwordVariable: 'PASSWORD'
                )]) {
                    bat """
                        net use \\\\192.168.1.225\\size-4t-sub-2t1-dellpc-smb /user:%USERNAME% %PASSWORD% /persistent:no
                        if not exist "\\\\192.168.1.225\\size-4t-sub-2t1-dellpc-smb\\cloud-devops-ai\\application\\microServices\\hello-world\\webapp.war" (
                            echo Error: webapp.war not found
                            exit 1
                        )
                        docker build -t tomcat:v14 "\\\\192.168.1.225\\size-4t-sub-2t1-dellpc-smb\\cloud-devops-ai\\application\\microServices\\hello-world"
                        docker run -d -p 8011:8080 --name tomcatv14 tomcat:v14
                        net use \\\\192.168.1.225\\size-4t-sub-2t1-dellpc-smb /delete
                    """
                }
            }
        }
    }
}