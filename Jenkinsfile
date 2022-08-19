pipeline {
    agent {
        node {
            label 'slave01'
        }
    }
    parameters {
    	string(name: 'serverIP', defaultValue: 'None', description: 'Enter Server IP ')
	string(name: 'servername', defaultValue: 'None', description: 'Enter Ansible slave name ')
	password(name: 'dockerpass', description: 'Enter docker login password ')	    
    }
    stages {
        
	stage('Remove dockers'){
	    steps {
		sh "if [ `sudo docker ps -a -q|wc -l` -gt 0 ]; then sudo docker rm -f \$(sudo docker ps -a -q);fi"
		}
	}
	stage('Build'){
	    steps {
		    sh "cd ${WORKSPACE}"
		    sh "sudo docker build -t devopsdemo ."
	   }
	}
	stage('Docker Push'){
		steps {
		    sh '''
		    echo "sudo docker login --username debasis-gif --password ${dockerpass}"
                    echo "sudo docker push debasis-gif/devopsdemo:latest"
		    '''
	        }
	}
	stage('Configure servers with Docker and deploy website') {
            	steps {
			sh '''
			echo "ansible-playbook docker.yaml -e hostname=${servername}"
			'''
            	}
        }
	stage('Install Chrome browser') {
            	steps {
                	sh '''
			echo "ansible-playbook chrome.yaml -e hostname=localhost"
			'''
            	}
        }
	stage ('Testing'){
		steps {
			sh "sudo apt install python3-pip -y"
			sh "pip3 install selenium"
			//sh "python3 selenium_test.py ${params.serverIP}"
		}
	}
    }
}
