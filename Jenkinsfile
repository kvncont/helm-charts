pipeline{
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: "15"))
        disableConcurrentBuilds()
        timeout(time: 15, unit: "MINUTES")
        timestamps()
    }
    parameters {
        choice(name: "CHART_NAME", choices: ["hello-world", "prometheus", "report-service"], description: "Choice the chart that you want to upgrade")
        string(name: "NAMESPACE", defaultValue: "default", description: "Namespace")
        choice(name: "ENVIRONMENT", choices: ["AKS_PRO_EastUS2", "AKS_PRO_CentralUS"], description: "Choice the region that you want to deploy de app")
        booleanParam(name: "ROLLBACK", defaultValue: true, description: "Do you want to rollback in case of any error?")
    }
    environment {
        REPOSITORY_NAME = "kvncont-charts"
        REPOSITORY_URL = "https://github.com/kvncont/helm-charts.git"
    }
    stages{
        stage("Helm - Upgrade") {
            steps {
                echo "Switch context"
                echo "kubectl config use-context ${ENVIRONMENT}"
                sh """
                    helm repo add ${REPOSITORY_NAME} ${REPOSITORY_URL}
                    helm repo update
                """
                echo "helm upgrade --install ${CHART_NAME} ${CHART_NAME} --namespace ${NAMESPACE}"
            }
        }
        stage("Test Deployment") {
            steps {
                echo "Testing application..."
            }
        }
        stage("Rollback") {
            when { expression { params.ROLLBACK == "true" } }
            steps {
                echo "Executing rollback..."
            }
        }
    }
}