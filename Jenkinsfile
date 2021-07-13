pipeline {
  agent {
    docker {
      label 'infrastructure-uge-submit-p01'
    }

  }

  parameters {
        string(name: 'NODE_NAME', defaultValue: '', description: 'ex) gen-uge-archive-p123')
        string(name: 'TICKET_ID', defaultValue: '', description: 'INFR-1234')
    }
  stages {
    stage('main') {
      agent {
        docker {
          image 'docker-infrapub.artifactory.ihme.washington.edu/selenium_chrome'
        }

      }
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
            
            echo $(whoami)

            # Install requires python package for api
            pip3 install --usaer requests

            # Making saving directory
            mkdir -p /mnt

            # Uploading screenshot to the ticket
            python3 src/main.py \
                  --username "${IDRAC_USERNAME}" \
                  --password "${IDRAC_PASSWORD}" \
                  --nodename "${NODE_NAME}" \
                  --ticket "${TICKET_ID}"
          '''
        }
      }
    }

  }
}
