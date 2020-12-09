node {
    stage('checkout') {

        sh '''
	            mkdir ${JENKINS_HOME}/init.groovy.d/
		    git clone https://github.com/zeroisme/jenkins_config_as_code.git ${JENKINS_HOME}/jenkins_config
			mv ${JENKINS_HOME}/jenkins_config/src/*.groovy ${JENKINS_HOME}/init.groovy.d/

		'''
    }

    stage('run consul template'){
        sh '''
			/var/jenkins_home/consul-template \
			  -consul-addr "$CONSUL_ADDR" \
			  -vault-addr "$VAULT_ADDR" \
			  -config "jenkins_config.hcl" \
			  -once        
        '''
    }

    stage('update credentials') {
        load("/var/jenkins_home/init.groovy.d/credentials.groovy")
    }

    stage('update k8s') {
        load("/var/jenkins_home/init.groovy.d/kubernetes.groovy")
    }

}
