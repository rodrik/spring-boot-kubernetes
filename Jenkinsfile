node {
    docker.withRegistry('https://hub.docker.com/', 'docker-hub-credentials') {
    	def mvnHome
    	
    	stage('checkout'){
        
        mvnHome = tool 'M3'
        git url: 'https://github.com/<user>/workplace.git'
    
        sh "git rev-parse HEAD > .git/commit-id"
        def commit_id = readFile('.git/commit-id').trim()
        println commit_id
        }
    
    dir('testnewproject'){
        stage('build'){
	        sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean install"
	        sh 'docker login --username <userName> --password <password>'
	        sh ("docker build -t testnewproject .")
	        sh ("docker tag  testnewproject prakashg84/test:testnewproject")
	        sh ("docker push <repo>/test:testnewproject")
    	}
    	
    	stage('create docker image'){
		        sh 'docker login --username <user> --password <user>'
		        sh ("docker build -t address-rest-api .")
		        sh ("docker tag  address-rest-api <repo>/test:address-rest-api")
    	}
    	
    	stage('push docker image'){
			sh ("docker push prakashg84/test:testnewproject")
    	}
    	
    	stage('create deployment'){
    	    sh 'kubectl delete deployments testnewprojectapi || true' 
	    sh 'kubectl create -f deployment.yaml --validate=false'
    	}
    	
    	stage('create service'){
	    sh 'kubectl delete services testnewprojectapiservice || true'
	    sh 'kubectl create -f services.yaml --validate=false'
    	}
    }
    }
}
