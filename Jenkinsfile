pipeline {
    agent any

    environment {
        TARGET_URL = "www.google.com"
        PORT = "443"
        OUTPUT_DIR = "certs"
        CERT_FILE = "${OUTPUT_DIR}\\${TARGET_URL}.pem" // 副檔名改為 .pem
    }

    stages {
        stage('切換編碼') {
            steps {
                bat 'chcp 65001'
            }
        }

        stage('建立資料夾') {
            steps {
                bat 'if not exist %OUTPUT_DIR% mkdir %OUTPUT_DIR%'
            }
        }

        stage('顯示資料夾內容') {
            steps {
                bat 'dir %OUTPUT_DIR%'
            }
        }

        stage('取得憑證鏈') { // 名稱也可改為取得憑證鏈
            steps {
                script {
                    try {
                        bat """
                        echo | "D:/Program Files/OpenSSL-Win64/bin/openssl" s_client -servername %TARGET_URL% -connect %TARGET_URL%:%PORT% -showcerts > %CERT_FILE% || exit /b 0
                        """
                    } catch (err) {
                        echo "❌ 憑證鏈下載失敗，錯誤訊息：${err}"
                        error "中止 Pipeline"
                    }
                }
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: "${OUTPUT_DIR}/*.pem", fingerprint: true // 改為 .pem
        }
    }
}
