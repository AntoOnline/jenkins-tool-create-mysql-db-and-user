pipeline {
    agent any

    environment {
        MYSQL_ROOT_USERNAME = credentials('mysql-root-username')
        MYSQL_ROOT_PASSWORD = credentials('mysql-root-password')
    }

    parameters {
        string(name: 'STACK_CODE', defaultValue: 'my-stack', description: 'The code for the stack')
        string(name: 'APP_CODE', defaultValue: 'my-app', description: 'The code for the app')
        string(name: 'ENCODING', defaultValue: 'utf8mb4', description: 'The encoding for the database')
        string(name: 'MAX_CONNECTIONS', defaultValue: '50', description: 'The max concurrent connections a user can have')
        string(name: 'MYSQL_HOST', defaultValue: 'localhost', description: 'The host address of mysql')
        string(name: 'MYSQL_PORT', defaultValue: '3306', description: 'The port of mysql')
    }

    stages {
        stage('Check MySQL client') {
            steps {
                script {
                    def osName = sh(script: "uname -s", returnStdout: true).trim()
                    if (osName == 'Linux') {
                        sh 'dpkg -s mysql-client || (sudo apt-get update && sudo apt-get install mysql-client -y)'
                    } else {
                        error "Unsupported operating system: ${osName}"
                    }
                }
            }
        }
        stage('Generate random password') {
            steps {
                script {
                    def charset = ('abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*()_+-=[]{}|;:<>,.?/~`').toCharArray()
                    def password = ""
                    (1..20).each {
                      password += charset[new Random().nextInt(charset.size())]
                    }
                    env.MYSQL_PASSWORD = password
                    echo "Generated password: ${password}"
                }
            }
        }
        stage('Create MySQL user and database') {
            steps {
                script {
                    def db_name = "${params.STACK_CODE.replaceAll('-','_')}_${params.APP_CODE.replaceAll('-','_')}"
                    sh "mysql -h ${params.MYSQL_HOST} -P ${params.MYSQL_PORT} -u ${MYSQL_ROOT_USERNAME} -p${MYSQL_ROOT_PASSWORD} -e 'CREATE DATABASE IF NOT EXISTS ${db_name} CHARACTER SET ${params.ENCODING};'"
                    sh "mysql -h ${params.MYSQL_HOST} -P ${params.MYSQL_PORT} -u ${MYSQL_ROOT_USERNAME} -p${MYSQL_ROOT_PASSWORD} -e 'CREATE USER ${db_name}@\"%\" IDENTIFIED BY \"${env.MYSQL_PASSWORD}\" WITH MAX_USER_CONNECTIONS ${params.MAX_CONNECTIONS};'"
                    sh "mysql -h ${params.MYSQL_HOST} -P ${params.MYSQL_PORT} -u ${MYSQL_ROOT_USERNAME} -p${MYSQL_ROOT_PASSWORD} -e 'FLUSH PRIVILEGES;'"
                }
            }
        }
    }
}
