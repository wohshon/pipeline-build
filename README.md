## sample pipeline build for ocp 3.7

This build config sets up a build pipeline to demo a simplified CICD flow

  * create a ci-cd project as a base project to run your jenkins

	```
	oc new-project ci-cd
	
	oc create -f sample-buildconfig.yaml
	```

  * create a 'dev' project

	```
	oc new-project myapp-dev
	
	oc policy add-role-to-user edit system:serviceaccount:ci-cd:default -n app1-dev
	
	oc new-build --name=nodejs-demo openshift/nodejs~https://github.com/wohshon/ocp-demo
	
	oc start-build nodejs-demo --follow

	oc new-app --image-stream=nodejs-demo:latest
	
	oc expose svc nodejs-demo
	```
  * Jenkins Plugin
  
    This demo uses the bindCredential Plugin, by default it is not installed. You have to install it manually. Some other plugins may be required to be upgraded as well 

  * Start the pipeline build, the code will be deployed and promoted in the following flow

	` app1-dev -> app1-qa -> app1-prod`
