pipeline {
  agent { node { label 'nodejs-apic' } }
  stages {
    stage('Validate API Lint') {
      steps {
        echo 'Hello-world'
        echo env.BRANCH_NAME


        echo '############################################################################################################################'
        echo '######################################      Validate API & Product YAMLS         ###########################################'
        echo '############################################################################################################################'

        // print the version
        sh "apic-slim version"

        //Validate the deployment yamls in cli (this does basix syntax and logic error checking)
        sh "apic-slim validate hello-world-product.yaml"
        
      }
    }
  }
}
