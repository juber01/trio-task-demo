pipeline{
        agent any
        stages{
            stage('On Jenkins Server'){
                steps{
                    sh '''
                    echo "hello World"
                    '''
                }
            }
            stage('On app Server'){
                steps{
                    sh '''
                    ssh -i "~/.ssh/id_rsa" jenkins@10.154.0.26 << EOF
                    docker image pull juber81/mynginx
                    docker image pull juber81/flask-app
                    docker image pull juber81/mysql
                    docker network create trio
                    docker run -d --network trio --name mysql juber81/mysql:latest
                    docker run -d --network trio --name flask-app juber81/flask-app:latest
                    docker run -d --network trio --name mynginx juber81/mynginx:latest
                    '''
                }
            }
        }
}
