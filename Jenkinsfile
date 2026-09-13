pipeline {
    agent any

    environment {
        M2_HOME = "/usr/share/maven" // Update this if needed
        PATH = "$M2_HOME/bin:$PATH"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/nimbuswiztech/maven_webapp.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Run Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Instsall app') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Approval') {
            steps {
                script {
                    def decision = input(
                        message: 'Approve to continue or Deny to stop',
                        parameters: [
                            choice(
                                name: 'DECISION',
                                choices: ['Approve', 'Deny'],
                                description: 'Select your decision'
                            )
                        ]
                    )

                    if (decision == 'Deny') {
                        error('❌ Deployment denied. Pipeline stopped.')
                    }

                    echo '✅ Approved. Continuing immediately...'
                }
            }
        }

        
        stage('Deploy') {
            steps {
                sshagent(['tomcat']) {
                    sh 'scp -o StrictHostKeyChecking=no target/demo.war ubuntu@13.201.230.202:/home/ubuntu/'
                    sh 'ssh ubuntu@13.201.230.202 "sudo mv /home/ubuntu/demo.war /opt/tomcat/webapps/"'
                }
            }
        }
    }

    post {
        success {
            emailext(
                to: 'chaitrae1688@gmail.com',
                subject: "Jenkins Success - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                    Hello,

                    Jenkins build has been successfully deployed.

                    Job: ${env.JOB_NAME}
                    Build Number: ${env.BUILD_NUMBER}

                    Regards,
                    Jenkins
                """
            )
        }

        failure {
            emailext(
                to: 'chaitrae1688@gmail.com',
                subject: "Jenkins Failed - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                    Hello,

                    Jenkins build has failed.

                    Job: ${env.JOB_NAME}
                    Build Number: ${env.BUILD_NUMBER}

                    Please check the Jenkins console output.

                    Regards,
                    Jenkins
                """
            )
        }
    }
}
