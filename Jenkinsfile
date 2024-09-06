pipeline {
    agent {
        docker {
            image 'alpine/helm:3.8.0'  // Image with Helm pre-installed
        }
    }

    stages {
        stage('Initialize Helm') {
            steps {
                script {
                    // Add the InfluxData Helm repo
                    sh 'helm repo add influxdata https://helm.influxdata.com/'
                    sh 'helm repo update'
                }
            }
        }

        stage('Deploy InfluxDB') {
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

    post {
        always {
            script {
                // Optionally check the status of the deployment
                sh 'kubectl get all -n monitoring'
            }
        }
    }
}
