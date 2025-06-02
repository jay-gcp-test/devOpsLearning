pipeline {
    agent any

    environment {
        PROJECT_ID = 'diesel-acolyte-460018-b9'
        CLUSTER_NAME = 'test-cluster-1'
        CLUSTER_ZONE = 'us-central1'
        GCP_CREDS = credentials('My First Project')

    }

    stages {
        stage ('Build Helm Chart')
        {
            steps {
                sh """
                    helm upgrade ./auto-nginx
                    helm install auto-nginx ./auto-nginx -n test-env
                """
            }
        }
    }
}