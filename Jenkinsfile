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
        string(name: "ARGS", defaultValue: "", description: "Insert arguments in case that you need it. Example (--set image.tag=1.0)")
        choice(name: "ENVIRONMENT", choices: ["AKS_PRO_EastUS2", "AKS_PRO_CentralUS"], description: "Select the cluster that you want to deploy the app")
        booleanParam(name: "ROLLBACK", defaultValue: true, description: "Do you want to rollback in case of any error?")
    }
    stages{
        stage("Helm - Upgrade") {
            steps {
                echo "Switch context"
                echo "kubectl config use-context ${ENVIRONMENT}"
                echo "helm upgrade --install ${CHART_NAME} ${CHART_NAME} --namespace ${NAMESPACE} ${ARGS}"
            }
        }
        stage("Test Deployment") {
            steps {
                echo "Testing application..."
            }
        }
        stage("Rollback") {
            when { expression { params.ROLLBACK == true } }
            steps {
                echo "Executing rollback..."
            }
        }
    }
}