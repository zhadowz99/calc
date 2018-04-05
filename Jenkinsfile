node('jenkins-agent-go-1') {
    
    try{        
        ws("${JENKINS_HOME}/jobs/${JOB_NAME}/builds/${BUILD_ID}/") {
                withEnv(["GOPATH=${JENKINS_HOME}/jobs/${JOB_NAME}/builds/${BUILD_ID}"]) {
                env.PATH="${GOPATH}/bin:$PATH"
                
                stage('Checkout'){
                    echo 'Checking out SCM'
                    checkout scm
                }
                
                stage('Initialize + Check Env'){
                    echo 'Initializing + Checking Environment...'
                    def root = tool name: 'Go 1.8', type: 'go'
                    withEnv(["GOROOT=${root}", "PATH+GO=${root}/bin"]) {
                    sh 'go version'

                    echo 'Pulling Dependencies'
            
                    sh 'go get -u github.com/golang/dep/cmd/dep'
                    sh 'go get -u github.com/golang/lint/golint'
                    sh 'go get github.com/tebeka/go2xunit'
                    
                    //or -update
                    sh 'cd ${GOPATH}/src/cmd/project/ && dep ensure' 
                    }
                }
        
                stage('Test'){
                    
                    //List all our project files with 'go list ./... | grep -v /vendor/ | grep -v github.com | grep -v golang.org'
                    //Push our project files relative to ./src
                    sh 'cd $GOPATH && go list ./... | grep github.com > projectPaths'
                    
                    //Print them with 'awk '$0="./src/"$0' projectPaths' in order to get full relative path to $GOPATH
                    def paths = sh returnStdout: true, script: """awk '\$0="./src/"\$0' projectPaths"""
                    
                    echo 'Vetting'

                    sh """cd $GOPATH && go tool vet ${paths}"""

                    echo 'Linting'
                    sh """cd $GOPATH && golint ${paths}"""
                    
                    echo 'Testing'
                    sh """cd $GOPATH && go test -race -cover ${paths}"""
                }            
            }
        }
    }catch (e) {
        
    } finally {
        
    }
}
