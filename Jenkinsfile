pipeline {

    agent any

    parameters {
        choice (
            name: 'ACTION',
            choices: ['apply','delete'],
            description: 'Do you want to apply or delete the kluster?'
        )
    }

    environment {
        // KUBECONFIG = credentials('kube-creds')
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
                message "Sure you want to apply changes to this cluster?"
            }

            steps {
                withCredentials([string(credentialsId: 'kube-creds', variable: 'KUBECONFIG')]) {
                    sh 'echo $KUBECONFIG | base64 -d > kubeconfig.yaml'
                    sh 'kubectl --kubeconfig=kubeconfig.yaml get pods'
                    sh """
                    kubectl --kubeconfig=kubeconfig.yaml ${ACTION} -f mongo-secret.yaml -n ${NAMESPACE}
                    kubectl --kubeconfig=kubeconfig.yaml ${ACTION} -f mongo-configmap.yaml -n ${NAMESPACE}
                    kubectl --kubeconfig=kubeconfig.yaml ${ACTION} -f mongo-deployment.yaml -n ${NAMESPACE}
                    kubectl --kubeconfig=kubeconfig.yaml ${ACTION} -f mongo-express.yaml -n ${NAMESPACE}
                    """
                }
            }
        }
    }
}
