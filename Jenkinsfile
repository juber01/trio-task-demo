pipeline{
        agent any
        stages{
            stage('Build Images'){
                steps{
                    sh '''
                    docker build -t juber81/mysql:latest ./db
                    docker build -t juber81/mysql:${BUILD_NUMBER} ./db
                    docker build -t juber81/flask-app:latest ./flask-app
                    docker build -t juber81/flask-app:${BUILD_NUMBER} ./flask-app
                    docker build -t juber81/mynginx:latest ./nginx
                    docker build -t juber81/mynginx:${BUILD_NUMBER} ./nginx
                    '''
                }
            }
            stage('Push Images'){
                steps{
                    sh '''
                    docker push -t juber81/mysql
                    docker push -t juber81/mysql:${BUILD_NUMBER}
                    docker push -t juber81/flask-app:latest
                    docker push -t juber81/flask-app:${BUILD_NUMBER}
                    docker push -t juber81/mynginx:latest
                    docker push -t juber81/mynginx:${BUILD_NUMBER}
                    '''
                }
            }
            stage('Cleanup Jenkins'){
                steps{
                    sh '''
                    docker rmi -t juber81/mysql
                    docker rmi -t juber81/mysql:${BUILD_NUMBER}
                    docker rmi -t juber81/flask-app:latest
                    docker rmi -t juber81/flask-app:${BUILD_NUMBER}
                    docker rmi -t juber81/mynginx:latest
                    docker rmi -t juber81/mynginx:${BUILD_NUMBER}
                    '''
                }
            }
            stage('Deploy Containers'){
                steps{
                    sh '''
                    ssh -i "~/.ssh/id_rsa" jenkins@10.154.0.26 << EOF
                    docker stop mysql && (docker rm mysql) || (docker rm mysql $$ sleep 1 || sleep 1)
                    docker stop mynginx && (docker rm mynginx) || (docker rm mynginx $$ sleep 1 || sleep 1)
                    docker stop flask-app && (docker rm flask-app) || (docker rm flask-app $$ sleep 1 || sleep 1)
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
