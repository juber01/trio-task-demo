pipeline{
        agent any
        stages{
            stage('Build Images'){}
            stage('Push Images'){}
            stage('Deploy Containers'){
                steps{
                    sh '''
                    ssh -i "~/.ssh/id_rsa" jenkins@10.154.0.26 << EOF
                    docker stop mynginx
                    docker stop mysql
                    docker stop flask-app
                    docker rm mynginx
                    docker rm mysql
                    docker rm flask-app
                    docker image pull juber81/mynginx
                    docker image pull juber81/flask-app
                    docker image pull juber81/mysql
                    docker network create trio
                    docker run -d --network trio --name mysql juber81/mysql:latest
                    docker run -d --network trio --name flask-app juber81/flask-app:latest
                    docker run -d -p 80:80 --network trio --name mynginx juber81/mynginx:latest
                    '''
                }
            }
        }
}
