pipeline {
 agent any
 
 stages {
	stage('clone'){
		steps {
			echo 'Cloning source code'
			git branch:'main', url: 'https://github.com/MrOwl715/.net-project.git'
		}
	} // end clone

  } // end stages
}//end pipeline
