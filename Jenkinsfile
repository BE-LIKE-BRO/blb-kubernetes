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
                withKubeConfig([credentialsId: 'kube-creds', serverUrl: '34.194.223.212:6443']) {
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
