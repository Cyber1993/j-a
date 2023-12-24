#!groovy
//  groovy Jenkinsfile
properties([disableConcurrentBuilds()])\

pipeline  {
        agent { 
           label ''
        }

    options {
        buildDiscarder(logRotator(numToKeepStr: '10', artifactNumToKeepStr: '10'))
        timestamps()
    }
    stages {
        stage("Git clone") {
            steps {
                sh '''
                cd /var/lib/jenkins/workspace/ansible
                git clone https://github.com/qwertynebot/ansible
                '''
            }
        }    
        stage("Build") {
            steps {
                sh '''
                cd /var/lib/jenkins/workspace/ansible/ansible.j/Ansible
                docker build -t darkne24/ansible .
                '''
            }
        } 
        stage("Postgres") {
            steps {
                sh '''
                docker run \
                --name ansible \
                -d darkne24/ansible
                '''
            }
        }
        stage("docker login") {
            steps {
                echo " ============== docker login =================="
                withCredentials([usernamePassword(credentialsId: 'DockerHub-Credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh '''
                    docker login -u $USERNAME -p $PASSWORD
                    '''
                }
            }
        }
        stage("docker push") {
            steps {
                echo " ============== pushing image =================="
                sh '''
                docker push darkne24/ansible
                '''
            }
        }
    }
}
