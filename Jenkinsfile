pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                echo 'Building the application...'
                sh 'ls -la'
            }
        }

        stage('Test') {
            steps {
                echo 'Pipeline is working!'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Testing WildFly connection...'
                withCredentials([usernamePassword(credentialsId: 'wildfly-creds', usernameVariable: 'WF_USER', passwordVariable: 'WF_PASS')]) {
                    sh '''
                    /opt/wildfly-cli/bin/jboss-cli.sh \
                      --connect \
                      --controller=13.48.29.172:9990 \
                      --user=$WF_USER \
                      --password=$WF_PASS \
                      --command=":read-attribute(name=server-state)"
                    '''
                }
            }
        }
    }
}

