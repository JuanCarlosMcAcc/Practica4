pipeline{
    agent{
        kubernetes{
            defaultContainer 'jdk'
            yaml '''
apiVersion: v1
kind: Pod
spec:
    securityContext:
        runAsUser: 1001
    containers:
      - name: jdk
        image: docker.io/eclipse-temurin:18.0.2.1_1-jdk
        command:
          - sleep
        args:
          - infinity
      - name: podman
        image: quay.io/containers/podman:v4.2.0
        command:
          - sleep
        args:
          - infinity
        securityContext:
          runAsUser: 0
          privileged: true
    imagePullSecrets:
      - name: master-acr-credentials
'''
        }
    }
    stages{
        stage("Prepare Environment"){
            steps{
                echo "-=- Prepare build environment -=-"
                sh 'java -version'
                container('podman'){
                    sh 'podman --version'
                }
                container('aks'){
                    withCredentials([
                            usernamePassword(
                                credentialsId: 'sp-terraform-credentials',
                                usernameVariable: 'AAD_SERVICE_PRINCIPAL_CLIENT_ID',
                                passwordVariable: 'AAD_SERVICE_PRINCIPAL_CLIENT_SECRET'),
                            string(crentialsId: 'aks-tenant', variable: 'AKS_TENANT'),
                            string(crentialsId: 'aks-resource-group', variable: 'AKS_RESOURCE_GROUP'),
                            string(crentialsId: 'aks-name', variable: 'AKS_NAME')]) {
                        sh "az login --service-principal --username ${AAD_SERVICE_PRINCIPAL_CLIENT_ID}"
                        sh "az aks get-credentials --resource-group ${AKS_RESOURCE_GROUP} --name ${AKS_NAME}"
                        sh 'kubelogin convert-kubeconfig -l spn'
                        sh 'kubectl version'
                    }
                }
            }
        }
        stage("Compile"){
            steps{
                echo "-=- Prepare build environment -=-"

            }
        }
    }

}
