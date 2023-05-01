pipeline {

    agent any

    environment {
        KUBECONFIG = credentials('kube-creds')
        NAMESPACE = "nobus-cloud-demo"
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
                    kubectl apply -f mongo-secret.yaml -n ${NAMESPACE}
                    kubectl apply -f mongo-configmap.yaml -n ${NAMESPACE}
                    kubectl apply -f mongo-deployment.yaml -n ${NAMESPACE}
                    kubectl apply -f mongo-express.yaml -n ${NAMESPACE}
                """
            }
        }
    }
}
