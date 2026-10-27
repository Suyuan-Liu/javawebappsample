node {
    withEnv([
        'AZURE_SUBSCRIPTION_ID=4a107f07-fff5-43b1-803f-a0d3ff50f94e',
        'AZURE_TENANT_ID=9f07f7fd-6ea2-434f-8a29-bee7789334ec'
    ]) {
        stage('Checkout') {
            checkout scm
        }

        stage('Build') {
            sh 'mvn -B clean package'
        }

        stage('Deploy') {
            withCredentials([usernamePassword(
                credentialsId: 'AzureServicePrincipal',
                passwordVariable: 'AZURE_CLIENT_SECRET',
                usernameVariable: 'AZURE_CLIENT_ID'
            )]) {
                sh '''
                    set +x
                    export AZURE_CONFIG_DIR="$WORKSPACE/.azure"
                    az login --service-principal \
                      --username "$AZURE_CLIENT_ID" \
                      --password "$AZURE_CLIENT_SECRET" \
                      --tenant "$AZURE_TENANT_ID" \
                      --output none
                    az account set --subscription "$AZURE_SUBSCRIPTION_ID"
                    az webapp deploy \
                      --resource-group jenkins-get-started-rg \
                      --name suyuan-ce8-jenkins-20261027 \
                      --src-path target/calculator-1.0.war \
                      --type war
                '''
            }
        }
    }
}
