# ICD0029
Container based development using OpenShift platform and k8s.


## First chapter
Talked about basic things. How to set up an project setup.

Every development should start with setting up basic things.

In this example we are looking `node.js`

1. Make sure to have nvm (Node version manger) that can be downloaded https://github.com/nvm-sh/nvm
2. You need to install `node.js` on your local env (macOS, linux, windows)
3. Git is essential to have your local env for version management
4. For publish your changes local -> Repository you should use GitHub, Bitbucket or whet ever you prefer to use. In this project we are using Github.
5. IDE is for VScode

If those things have been setup you can basically start develop.
First chapter also we covered version management. How to create branches, fork from another repository, merge requests etc. 

## Second chaper


### Introduction
Developer sandbox quick link is here https://developers.redhat.com/developer-sandbox

Described how pods logic work on OpenShift. It acts very similar like in k8s with Rancher where smallest deployable object is pod. Also pod will get its own IP address and storage from PVC or PV other vise you will lost your data. 

Basic work flow	
1. The developers commit work to a Git repository.
2. When ready to deploy their code, the developers use the OpenShift web console to create the application. The URL to the Git repository is one of the required parameters.
3. OpenShift retrieves the code from the Git repository and builds the application.
4. OpenShift deploys the application in a pod.

Its not covered yet where the images are pulled? Dockerhub? Or registry? Or RH manages it self?


Web console logic is same as Racnher where you don't need to use kubectl commands to operate.

RedHat calls as Projects for example rancher calls then namespaces.

Also what is cool and different about not sure, but it can monitor your repo. Usually we use some 3rd party plugins for example ArgoCD or Jenkins, Gitlab CI/CD.

### Deploy your first images to OpenShift

Our OpenShift lab environment is found following link
https://console-openshift-console.apps.na46r.prod.ole.redhat.com

After successful log in you must ensure that you will use right roles. Usually we just need Developer roles.

We will have to create new project. Call it whet ever you like to. Here we just call it `l6dev-deploy-app` 

OpenShift is provide multiple options for deployment. Here we just use from Git.

To get code from repository -> https://github.com/ashenell/DO101-apps


To fetch repository we will use method From Catalog
1. Languages we pick JavaScript and search builder
2. Create application
3. Git repo url https://github.com/ashenell/DO101-apps
4. Create a route to the application `Make sure the box is checked`

If build was successfull it should show completed. ALso from logs you can see the following steps from process. 

In some reason if you try to access to the routes link to validate that your application is running in Brave browser it gives an result that service is unavailable. If doing same steps with firefox everything is attended to be working properly. 

### Summary for chapter 2

It seems to be very similar with k8s with rancher CLI.
