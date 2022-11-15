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

'''
        }
    }
    stages{
        stage("Prepare Environment"){
            steps{
                echo "-=- Prepare build environment -=-"
                sh 'java -version'
                echo "Arriba se muestra la version de JAVA"
            }
        }
        stage("Compile"){
            steps{
                echo "-=- Prepare build environment -=-"
                sh 'podman -version'
            }
        }
    }

}
