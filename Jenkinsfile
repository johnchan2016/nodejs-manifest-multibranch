node {
    def app
		def projectName = 'nodejs-manifest'
		def projectBuildPath = "${projectName}/${APPENV}"
		def yamlPath = "manifests/overlays/${APPENV}"

    stage('Clone repository') {     
        checkout scm
    }

    stage('Update GIT') {
	script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    sh "git config user.email myhk2009@gmail.com"
                    sh "git config user.name johnchan2016"
										
                    sh """
			# create new kustomization.yaml
			mkdir -p $projectBuildPath
			mv $yamlPath/kustomization.yaml $projectBuildPath
			yq -i '.images[0].newName = "myhk2009/nodetest"' $projectBuildPath/kustomization.yaml
			yq -i '.images[0].newTag = "$DOCKERTAG"' $projectBuildPath/kustomization.yaml

			cat $projectBuildPath/kustomization.yaml

			# replace original kustomization.yaml
			mv $projectBuildPath/kustomization.yaml $yamlPath
			cat $yamlPath/kustomization.yaml						
			rm -r $projectBuildPath
		    """
										
                    sh "git add ."
                    sh "git commit -m 'Done by Jenkins Job changemanifest: $DOCKERTAG'"
                    sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/${projectName}.git HEAD:main"
                }
            }
        }
    }
}
