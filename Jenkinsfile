pipeline {
    agent {
        label 'master'
    }
    options {
        skipDefaultCheckout()
    }
    stages {
        stage("One big stage") {
            steps {
                sh "if [ -d /opt/tomcat/webapps/ROOT ]; then sudo rm -rf /opt/tomcat/webapps/ROOT fi"
                sh "if [ -f /opt/tomcat/webapps/ROOT.war ]; then sudo rm -f /opt/tomcat/webapps/ROOT.war fi"
                sh "cd /var/lib/jenkins/workspace/EasyPay"
                sh "gradle clean buil -x test"
                sh "sudo service postgresql reload"
                sh "sudo -u postgres psql postgres -c 'DROP DATABASE easypay_db;'"
                sh "sudo mv /var/lib/jenkins/workspace/EasyPay/build/libs/EasyPay-1.0-SNAPSHOT.war /opt/tomcat/webapps/ROOT.war"
                sh "sudo -u postgres psql postgres -c 'CREATE DATABASE easypay_db;'"
                sh "sudo -u postgres psql postgres -c 'GRANT ALL PRIVILEGES ON DATABASE easypay_db TO postgres;'"
                sh "sudo -u postgres psql easypay_db < /var/lib/jenkins/workspace/EasyPay/2.sql"
                sh "sudo systemctl restart tomcat"
            }
        }
    }
}

