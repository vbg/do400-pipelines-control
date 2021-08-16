pipeline {
  agent {
    node { label 'nodejs' } 
	}
  parameters {
       booleanParam( name: "RUN_FRONTEND_TEST", defaultValue: true )
    }
  stages {
	stage ('Checkout') {
           steps {
		git branch: 'main', url: 'https://github.com/vbg/do400-pipelines-control.git'
           }
	}
       stage ('Run Tests') {
          parallel {
	      stage ('Backend Tests') {
                 steps {
		    sh 'node ./backend/test.js'
                 }
	      } 
	      stage ('Frontend Tests') {
                when {
                    expression { params.RUN_FRONTEND_TEST }
                }
                steps {
		    sh 'node ./frontend/test.js'
                }
             }
         }
      }
     stage ('Deploy') {
        when {
           expression { env.GIT_BRANCH == "origin/main" }
           beforeInput true
        }
        input {
           message "Deploy the Application?"
        }
        steps {
           echo "Deploying..."
        }
     }
  }
}
