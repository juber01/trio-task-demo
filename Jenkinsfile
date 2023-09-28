pipeline{
        agent any
        stages{
            stage('Build Images'){
                steps{
                    sh '''
                    docker build -t gcr.io/lbg-mea-14/ja-mysql:latest ./db
                    docker build -t gcr.io/lbg-mea-14/ja-mysql:${BUILD_NUMBER} ./db
                    docker build -t gcr.io/lbg-mea-14/ja-flask-app:latest ./flask-app
                    docker build -t gcr.io/lbg-mea-14/ja-flask-app:${BUILD_NUMBER} ./flask-app
                    docker build -t gcr.io/lbg-mea-14/ja-mynginx:latest ./nginx
                    docker build -t gcr.io/lbg-mea-14/ja-mynginx:${BUILD_NUMBER} ./nginx
                    '''
                }
            }
            stage('Push Images'){
                steps{
                    sh '''
                    docker push gcr.io/lbg-mea-14/ja-mysql
                    docker push gcr.io/lbg-mea-14/ja-mysql:${BUILD_NUMBER}
                    docker push gcr.io/lbg-mea-14/ja-flask-app:latest
                    docker push gcr.io/lbg-mea-14/ja-flask-app:${BUILD_NUMBER}
                    docker push gcr.io/lbg-mea-14/ja-mynginx:latest
                    docker push gcr.io/lbg-mea-14/ja-mynginx:${BUILD_NUMBER}
                    '''
                }
            }
            stage('Cleanup Jenkins'){
                steps{
                    sh '''
                    docker rmi gcr.io/lbg-mea-14/ja-mysql
                    docker rmi gcr.io/lbg-mea-14/ja-mysql:${BUILD_NUMBER}
                    docker rmi gcr.io/lbg-mea-14/ja-flask-app:latest
                    docker rmi gcr.io/lbg-mea-14/ja-flask-app:${BUILD_NUMBER}
                    docker rmi gcr.io/lbg-mea-14/ja-mynginx:latest
                    docker rmi gcr.io/lbg-mea-14/ja-mynginx:${BUILD_NUMBER}
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
                    docker image pull gcr.io/lbg-mea-14/ja-mynginx
                    docker image pull gcr.io/lbg-mea-14/ja-flask-app
                    docker image pull gcr.io/lbg-mea-14/ja-mysql
                    docker network inspect trio && sleep 1 || docker network create trio
                    docker run -d --network trio --name mysql gcr.io/lbg-mea-14/ja-mysql:latest
                    docker run -d --network trio --name flask-app gcr.io/lbg-mea-14/ja-flask-app:latest
                    docker run -d -p 80:80 --network trio --name mynginx gcr.io/lbg-mea-14/ja-mynginx:latest
                    '''
                }
            }
        }
}
