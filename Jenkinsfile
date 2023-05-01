pipeline {

    agent any

    environment {
        KUBECONFIG = credentials('kube-creds')
        NAMESPACE-1 = "nobus-cloud-demo"
    }

    stages {

        stage('Checkout') {
            steps {
                deleteDir()
                checkout scm
            }
        }

         stage('Deploy Cluster') {
            input {
                message "Do you want to Deploy the updated cluster??"
            }

            steps {
                sh """
                    kubectl apply -f mongo-secret.yaml -n ${NAMESPACE-1}
                    kubectl apply -f mongo-configmap.yaml -n ${NAMESPACE-1}
                    kubectl apply -f mongo-deployment.yaml -n ${NAMESPACE-1}
                    kubectl apply -f mongo-express.yaml -n ${NAMESPACE-1}
                """
            }
        }
    }
}
