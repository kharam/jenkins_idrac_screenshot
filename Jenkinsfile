pipeline {
  agent {
    docker {
      image 'docker-infrapub.artifactory.ihme.washington.edu/selenium_chrome'
    }

  }

  parameters {
    parameters { string(name: 'NODE_NAME', defaultValue: '', description: '') }
    parameters { string(name: 'TICKET_ID', defaultValue: '', description: '') }
  }
  stages {
    stage('main') {
      steps {
        withCredentials([
          usernamePassword(credentialsId: 'ticket-machine.atlassian.user', usernameVariable: 'SERVICEDESK_USERNAME', passwordVariable: 'SERVICEDESK_PASSWORD'),
          usernamePassword(credentialsId: 'idrac.user.root', usernameVariable: 'IDRAC_USERNAME', passwordVariable: 'IDRAC_PASSWORD'),
        ]) {
          sh '''
            # Setting Environemnt Variables
            export SERVICEDESK_BASE_URL=https://help.ihme.washington.edu
            export SERVICEDESK_USERNAME=${SERVICEDESK_USERNAME}
            export SERVICEDESK_PASSWORD=${SERVICEDESK_PASSWORD}

            # Install requires python package for api
            pip3 install requests

            # Uploading screenshot to the ticket
            python3 /mnt/main.py \
                  --username ${IDRAC_USERNAME} \
                  --password ${IDRAC_PASSWORD} \
                  --nodename ${NODE_NAME} \
                  --ticket ${TICKET_ID}
          '''
        }
      }
    }

  }
}