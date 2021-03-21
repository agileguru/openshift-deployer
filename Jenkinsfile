node {
  
	// Checkout from SCM
  	stage ("Checkout") {
  		checkout scm
  	}
  // Read From Properties
	  projects = readFile("${env.BRANCH_NAME}-projects.properties") 
	  environments = readFile("${env.BRANCH_NAME}-environments.properties") 
	  commands = ['ReDeploy', 'Remove']
	  properties([
	       parameters([
	       choice( choices: commands,      description: 'Command',   name: 'Command'), 
  	       choice( choices: projects,      description: 'Service',   name: 'Service'), 
  	       choice( choices: environments,  description: 'Environment', name: 'Environment')
	       ]), 
	       pipelineTriggers([])
	  ])
  	stage ("${env.Command} ${env.Service} in ${env.Environment} " ) {
  	  String service = "${env.Service}";
  	  String namespace = "${env.Environment}";
  	  String command = "${env.Command}";
  	  
      if ( service != "" && namespace != "" ) { 
        if ( command.equals('ReDeploy') ) { 
            echo "Redeploy service " + service + " to " + namespace; 
            echo "oc --kubeconfig=${WORKSPACE}/config --context=${env.Environment} delete --ignore-not-found -f ${WORKSPACE}/projects/${env.Service}/"
            echo "oc --kubeconfig=${WORKSPACE}/config --context=${env.Environment} create -f ${WORKSPACE}/projects/${env.Service}/" 
        }
        if ( command.equals('Remove') ) { 
            echo "Remove service " + service + " to " + namespace; 
            echo "oc ${WORKSPACE}/config --context=${env.Environment} delete --ignore-not-found -f ${WORKSPACE}/projects/${env.Service}/"
        }
      }
  	}
}