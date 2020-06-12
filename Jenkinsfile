pipeline {
  agent { 
    node { label 'nodejs-apic' } 
  }
  environment {
    APIC_DEV_CREDS = credentials('jenkins-apic-dev-creds')
  }
  stages {
    stage('Validate API Lint') {
      steps {
        echo 'Hello-world'
        echo env.BRANCH_NAME


        echo '############################################################################################################################'
        echo '######################################      Validate API & Product YAMLS         ###########################################'
        echo '############################################################################################################################'

        // print the version
        //test
        sh "apic-slim version"

        //Validate the deployment yamls in cli (this does basix syntax and logic error checking)
        sh "apic-slim validate hello-world-product.yaml"
        
      }
    }
      stage('Login to APIC') {
      steps {
        // login

        echo '############################################################################################################################'
        echo '########################################ß      Login to APIC Connect         ################################################'
        echo '############################################################################################################################'

        sh "apic-slim login --username ${env.APIC_DEV_CREDS_USR} --password ${env.APIC_DEV_CREDS_PSW} --server https://mgmt.icp-proxy.project-fidelity-uk-3cd0ec11030dfa215f262137faf739f1-0000.eu-gb.containers.appdomain.cloud --realm provider/default-idp-2"
        
      }
    }
       

     stage('deploy-dev') {
         when {
          expression {
            env.BRANCH_NAME =~ /^.*(develop|PR).*$/
        }
       }
      steps {
        //deploy to environment
        echo '############################################################################################################################'
        echo '########################################      Deploy to Dev environment         ############################################ß'
        echo '############################################################################################################################'
        sh "apic-slim products publish hello-world-product.yaml --server https://mgmt.icp-proxy.project-fidelity-uk-3cd0ec11030dfa215f262137faf739f1-0000.eu-gb.containers.appdomain.cloud --org fidelity-integration --catalog fidelity-dev"
      }
    }
    stage('test-dev') {
        when {
          expression {
            env.BRANCH_NAME =~ /^.*(develop|PR).*$/
        }
       }

      steps {
        //test dev environment 
        echo '############################################################################################################################'
        echo '##########################################      Test Dev Environment       #################################################ß'
        echo '############################################################################################################################'

        sh "curl --request GET \
        --url https://gw.icp-proxy.project-fidelity-uk-3cd0ec11030dfa215f262137faf739f1-0000.eu-gb.containers.appdomain.cloud/fidelity-integration/fidelity-dev/hello-world/ \
        --header 'accept: application/json' --insecure --silent --fail"
        
      }
    }

    
     stage('deploy-st') {
       when {
          expression {
            env.BRANCH_NAME =~ /^.*(develop).*$/
        }
       }
      steps {
        echo '############################################################################################################################'
        echo '##########################################      Deploy to System Test         ##############################################'
        echo '############################################################################################################################'

        sh "apic-slim products publish hello-world-product.yaml --server https://mgmt.icp-proxy.project-fidelity-uk-3cd0ec11030dfa215f262137faf739f1-0000.eu-gb.containers.appdomain.cloud --org fidelity-integration --catalog fidelity-st"
      }
    }
    stage('test-st') {
        when {
          expression {
            env.BRANCH_NAME =~ /^.*(develop).*$/
        }
       }
      steps {

        echo '############################################################################################################################'
        echo '###############################################      Test System Test         ##############################################'
        echo '############################################################################################################################'
        //test st environment 
        sh "curl --request GET \
        --url https://gw.icp-proxy.project-fidelity-uk-3cd0ec11030dfa215f262137faf739f1-0000.eu-gb.containers.appdomain.cloud/fidelity-integration/fidelity-st/hello-world/ \
        --header 'accept: application/json' --insecure --silent --fail"
        
      }
    }
     stage('deploy-sit') {
       when {
          expression {
            env.BRANCH_NAME =~ /^.*(master).*$/
        }
       }
      steps {

        echo '############################################################################################################################'
        echo '###############################################      Deploy to SIT         #################################################'
        echo '############################################################################################################################'
        sh "apic-slim products publish hello-world-product.yaml --server https://mgmt.icp-proxy.project-fidelity-uk-3cd0ec11030dfa215f262137faf739f1-0000.eu-gb.containers.appdomain.cloud --org fidelity-integration --catalog fidelity-sit"
      }
    }
    stage('test-sit') {
       when {
          expression {
            env.BRANCH_NAME =~ /^.*(master).*$/
        }
       }
      steps {
        //test sit environment 

        echo '############################################################################################################################'
        echo '################################################         Test SIT         ##################################################'
        echo '############################################################################################################################'
        sh "curl --request GET \
        --url https://gw.icp-proxy.project-fidelity-uk-3cd0ec11030dfa215f262137faf739f1-0000.eu-gb.containers.appdomain.cloud/fidelity-integration/fidelity-sit/hello-world/ \
        --header 'accept: application/json' --insecure --silent --fail"
        
      }
    }
  }
}