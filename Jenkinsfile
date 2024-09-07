pipeline {
    agent none  // Define no agent globally to handle it at stage level

    stages {
        stage('Initialize Helm') {
            agent {
                docker {
                    image 'alpine/k8s:1.28.13'
                }
            }
            steps {
                withKubeConfig(credentialsId: 'microk8s') {
                    // Set both XDG_CONFIG_HOME and XDG_CACHE_HOME to writable directories
                    sh '''
                    export XDG_CONFIG_HOME=$PWD/.config
                    export XDG_CACHE_HOME=$PWD/.cache
                    mkdir -p $XDG_CONFIG_HOME $XDG_CACHE_HOME
                    helm repo add influxdata https://helm.influxdata.com/
                    helm repo update
                    '''
                }
            }
        }

        stage('Deploy InfluxDB') {
            agent {
                docker {
                    image 'alpine/k8s:1.28.13'
                }
            }
            steps {
                script {
                    sh 'helm install my-influxdb influxdata/influxdb --version 4.12.5'
                }
            }
        }
    }

}
