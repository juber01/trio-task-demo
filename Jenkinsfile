pipeline{
        agent any
        stages{
            stage('Build Images'){}
            stage('Push Images'){}
            stage('Cleanup Jenkins'){}
            stage('Deploy Containers'){
                steps{
                    sh '''
                    ssh -i "~/.ssh/id_rsa" jenkins@10.154.0.26 << EOF
                    if [docker stop mysql]; then
                        docker rm mysql
                    else
                        if [ docker rm mysql ]; then
                            sleep 1
                        else
                            sleep 1
                        fi
                    fi
                    if [docker stop mynginx]; then
                        docker rm mynginx
                    else
                        if [ docker rm mynginx ]; then
                            sleep 1
                        else
                            sleep 1
                        fi
                    fi
                    if [docker stop flask-app]; then
                        docker rm flask-app
                    else
                        if [ docker rm flask-app ]; then
                            sleep 1
                        else
                            sleep 1
                        fi
                    fi
                    docker image pull juber81/mynginx
                    docker image pull juber81/flask-app
                    docker image pull juber81/mysql
                    docker network inspect trio && sleep 1 || docker network create trio
                    docker run -d --network trio --name mysql juber81/mysql:latest
                    docker run -d --network trio --name flask-app juber81/flask-app:latest
                    docker run -d -p 80:80 --network trio --name mynginx juber81/mynginx:latest
                    '''
                }
            }
        }
}
