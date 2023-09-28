pipeline{
        agent any
        stages{
            stage('On Jenkins Server'){
                steps{
                    sh '''
                    docker login --username juber81 --password Litr@Pr0$
                    docker images
                    '''
                }
            }
            stage('On app Server'){
                steps{
                    sh '''
                    ssh -i "~/.ssh/id_rsa" jenkins@10.154.0.26 << EOF
                    rm -rf LBG-14-freestyle-project
                    git clone git@github.com:juber01/LBG-14-freestyle-project.git
                    echo "Hello, jenkins is working"
                    hostname
                    '''
                }
            }
        }
}
