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
                script {
                    // Set both XDG_CONFIG_HOME and XDG_CACHE_HOME to writable directories
                    sh '''
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
                    // Helm command to install or upgrade InfluxDB
                    sh '''
                    helm upgrade --install influxdb influxdata/influxdb \
                      --namespace monitoring \
                      --create-namespace \
                      --set persistence.enabled=true \
                      --set service.type=https://192.168.86.27:16443
                    '''
                }
            }
        }
    }

}
