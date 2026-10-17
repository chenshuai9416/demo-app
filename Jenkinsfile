pipeline {
    agent any

    environment {
        REGISTRY = "192.168.6.133:80"
        PROJECT = "library"
        IMAGE_NAME = "demo-app"
        TAG = "v${BUILD_NUMBER}"
    }

    stages {
        stage('1. Checkout') {
            steps {
                echo "拉取代码..."
                git 'https://github.com/chenshuai9416/demo-app.git'
            }
        }

        stage('2. Build Image') {
            steps {
                echo "构建 Docker 镜像..."
                sh "docker build -t ${REGISTRY}/${PROJECT}/${IMAGE_NAME}:${TAG} ."
            }
        }

        stage('3. Push Image') {
            steps {
                echo "推送镜像到 Harbor..."
                sh """
                    docker login ${REGISTRY} -u admin -p Harbor12345 --tls-verify=false || true
                    docker push ${REGISTRY}/${PROJECT}/${IMAGE_NAME}:${TAG} --tls-verify=false
                """
            }
        }

        stage('4. Deploy to K8s') {
            steps {
                echo "部署到 K8s 集群..."
                sh """
                    kubectl set image deployment/nginxweb nginxwebc=${REGISTRY}/${PROJECT}/${IMAGE_NAME}:${TAG} -n default
                    kubectl rollout status deployment/nginxweb -n default --timeout=90s
                """
            }
        }
    }
    
    post {
        success {
            echo "🎉 恭喜！CI/CD 流水线执行成功！新版本 ${TAG} 已上线！"
        }
    }
}
