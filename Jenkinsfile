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
            steps {
                withCredentials([file(credentialsId: 'kube-creds', variable: 'KUBECONFIG')]) {
                    sh """
                    kubectl ${ACTION} -f mongo-backend.yaml -n ${NAMESPACE}
                    kubectl ${ACTION} -f mongo-express.yaml -n ${NAMESPACE}
                    """
                }
            }
        }
    }
}
