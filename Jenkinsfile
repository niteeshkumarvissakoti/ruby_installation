pipeline {
    agent any

    environment {
        FILENAME = "ruby_install_status_${BUILD_NUMBER}.txt"
    }

    stages {
        stage('Verify Ruby') {
            steps {
                sh '''
                echo "Checking for Ruby installation..."

                if command -v ruby > /dev/null 2>&1; then
                    echo "Ruby is already installed."
                    ruby --version
                else
                    echo "Ruby not found. Installing Ruby..."
                    sudo yum install -y ruby
                fi
                '''
            }
        }

        stage('Write Status File') {
            steps {
                sh '''
                echo "Installation completed successfully for build $BUILD_NUMBER" > $FILENAME
                '''
            }
        }

        stage('Archive Status') {
            steps {
                archiveArtifacts artifacts: '${FILENAME}', allowEmptyArchive: false
            }
        }
    }
}
