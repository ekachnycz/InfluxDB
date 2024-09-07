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
                     sh 'export XDG_CONFIG_HOME=$PWD/.config && helm repo add influxdata https://helm.influxdata.com/ && helm repo update'
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
                      --set service.type=ClusterIP
                    '''
                }
            }
        }
    }

}
