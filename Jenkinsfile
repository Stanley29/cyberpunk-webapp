pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                echo 'Building the application...'
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                echo 'Pipeline is working!'
            }
        }

        stage('Deploy') {
            agent { label 'built-in' }
            steps {
                echo 'Deploying application to WildFly...'
                withCredentials([usernamePassword(credentialsId: 'wildfly-creds', usernameVariable: 'WF_USER', passwordVariable: 'WF_PASS')]) {
                    sh '''
                    WAR_FILE=target/cyberpunk-webapp.war

                    /opt/wildfly-cli/bin/jboss-cli.sh \
                      --connect \
                      --controller=13.48.29.172:9990 \
                      --user=$WF_USER \
                      --password=$WF_PASS \
                      --command="undeploy cyberpunk-webapp.war --keep-content"

                    /opt/wildfly-cli/bin/jboss-cli.sh \
                      --connect \
                      --controller=13.48.29.172:9990 \
                      --user=$WF_USER \
                      --password=$WF_PASS \
                      --command="deploy $WAR_FILE --force"
                    '''
                }
            }
        }
    }
}
