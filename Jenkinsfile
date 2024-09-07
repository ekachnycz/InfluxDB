pipeline {
    agent none  // Define no agent globally to handle it at stage level

    stages {
        stage('Initialize Helm') {
            agent {
                docker {
                    image 'alpine/helm:3.8.0'
                }
            }
            steps {
                script {
                    // Add the InfluxData Helm repo
                    sh 'helm repo add influxdata https://helm.influxdata.com/'
                    sh 'helm repo update'
                }
            }
        }

        stage('Deploy InfluxDB') {
            agent {
                docker {
                    image 'alpine/helm:3.8.0'
                }
            }
            steps {
                script {
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

    post {
        always {
            script {
                // Optionally check the status of the deployment
                sh 'kubectl get all -n monitoring'
            }
        }
    }
}
