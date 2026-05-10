pipeline {
    agent none

    stages {

        stage('Build') {
            agent { label 'slave1' }
            steps {
                sh 'mvn clean package -DskipTests'
                stash includes: 'target/*.war', name: 'warfile'
            }
        }

        stage('Prepare Artifact on Master') {
            agent { label 'built-in' }
            steps {
                unstash 'warfile'
                sh 'ls -l target'
            }
        }

        stage('Deploy to WildFly') {
            agent { label 'built-in' }
            steps {
                withCredentials([usernamePassword(credentialsId: 'wildfly-creds', usernameVariable: 'WF_USER', passwordVariable: 'WF_PASS')]) {
                    sh '''
                        /opt/wildfly-cli/bin/jboss-cli.sh \
                          --connect \
                          --controller=13.48.29.172:9990 \
                          --user=$WF_USER \
                          --password=$WF_PASS \
                          --command="undeploy cyberpunk-webapp.war --keep-content" || true

                        /opt/wildfly-cli/bin/jboss-cli.sh \
                          --connect \
                          --controller=13.48.29.172:9990 \
                          --user=$WF_USER \
                          --password=$WF_PASS \
                          --command="deploy target/cyberpunk-webapp.war --force"
                    '''
                }
            }
        }
    }
}
