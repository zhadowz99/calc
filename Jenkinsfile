node('jenkins-agent-go-1') {
  
  try{

    stage('Cleaning Workspace') {
        echo 'Initializing for clean build...'
        deleteDir()
    }
    
    stage('Initialize + Check Env') {
        echo 'Initializing + Checking Environment...'
	    def root = tool name: 'Go 1.8', type: 'go'
	    withEnv(["GOROOT=${root}", "PATH+GO=${root}/bin"]) {
            sh 'go version'
        }
    }

    stage('Checkout SCM') {
        echo 'Checkout SCM...'
        checkout scm
    }
  
    try{

        stage('Build') {
            echo 'Building Dependency...'
        }

        stage('Testing'){
            echo 'Testing...'
        }
	
    }finally{
        
    stage('SonarQube analysis') {
        
        def scannerHome = tool 'SonarQube Scanner';
        withSonarQubeEnv('SonarQube') {
          sh "${scannerHome}/bin/sonar-scanner"
        }
    }
    
  }

  }catch(e){

    throw e;
  }
}
