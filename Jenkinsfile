pipeline {
  agent {
    kubernetes {
        yamlFile "./k8s/build.yaml"
    }
  }

  environment {
      SERVICE = "erpnext-hlc"
      PROJECT = "halocom-io/${SERVICE}"
      GITHUB_REPO = "https://github.com/${PROJECT}"
      REGISTRY = "registry.digitalocean.com/halocom"
  }

  stages {
	stage('Pull new soure'){
		steps {
           timeout(10) {
             withKubeConfig([credentialsId: 'k8s-config-file']) {
               sh """
				   kubectl -n erpnext exec -it deployments/backend-frappe-deployment -- bash -c "cd apps/erpnext && git pull && git checkout ${BRANCH_NAME}"
               """
             }
           }
       }
	}

	stage('Migrate new soure'){
		steps {
           timeout(10) {
             withKubeConfig([credentialsId: 'k8s-config-file']) {
               sh """
				   kubectl -n erpnext exec -it deployments/backend-frappe-deployment -- bash -c "bench --site erpnext.halocom.io migrate"
               """
             }
           }
       }
	}
  }
}
