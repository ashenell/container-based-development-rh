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

## Third chapter

In chapter two we raised a question where images are pulled. OpenShift have built in container registry so no need to use DockerHub or some other tools for that.

For similar thing with k8s is kubectl instead OpenShift theres oc command line. 

Basic workflow

1. Update your code your local environment
2. Push changes to your repository (Here we use GitHub)
3. Trigger manual build process from web console
4. Build new container image
5. New deployment or pod
6. Old version of pod is terminated automatically

Automatic workflow using webhooks

1. Update your code your local environment
2. Push changes to your repository (Here we use GitHub)
3. Webhook triggers new build automatically through REST API
4. Build new container image
5. New deployment or pod
6. Old version of pod is terminated automatically

Main difference is that once webhooks are set it will does build process from you need to do something manually instead you can keep focusing on other important things.

### Updating an application

For operating with OC you need to download tools for. Follow the instructions and make sure to download whet ever OS your are using MacOS or what ever.

Once client is downloaded you must ensurer that MacOS zsh will have access to oc commands. Then extract your zip anywhere you like and describe path `export PATH="$PATH:/Users/rauno/Downloads/test_oc"` with `oc version` it should give client version number.

To login using `oc` you must copy your login information from RHOCP.

If there was a successful login it should tell you 
`Using project "l6dev".
Welcome! See 'oc help' to get started.`

To crate now project follow next instructions

1. `oc new-project some-random-name`

To push changes we must make some changes in to our code. We are carate new branch for that.

`oc expose svc/version` will make your application to expose and you can check it out from url. Similar thing in workspace.

So far it was for manual updates. For automate this process you should open your git repository and must follow next steps

1. Settings
2. Webhooks
3. Add webhook
4. Get payload 
5. Content type use application/json
6. Add webhook

Github will make a ping request and it should say successful attempt.

Now if you will push changes it should trigger automatic build process.

### Configure application secrets

In this chapter we are speaking about config maps and secrets. How those things ise used etc.

In here we use base64, but be careful and in real scenario use something stronger for example rsa254. Not sure why be use base64 in this example. But so be it.

Basic flow is

1. Create your YAML config file
2. Add values
```yml
kind: ConfigMap 1
apiVersion: v1
data: 2
    username: myuser
    password: mypass
metadata:
    name: myconf 3
```
3. Create ConfigMaps and secrets in OpenShift
4. Inject ConfigMaps and screts as env variables
5. Push application Pod
6. Your application now will have access to your variables

In this lesson we will add some secrets and yaml files to get API key and also we are interested to setup config maps.


**There are some sort of error just giving an logical url path to repo to monitor. Som reason it wont give access idk.**

-- Problem actually was there that i attempt to open wrong link. You must make sure to open proper rote. For doing it just open topology and open your deployed application and find route and do clean link copy.

Deleting pod means that it will automatically builds the new one. Useful if you did made changes on secrets or in config maps.


### Connecting to Database

Database connection string is 
```postgresql://username:password@dbservice:port/dbname```
It must contain following

1. ```username``` - The database user name for accessing the database
2. ```password```- The password for accessing the database
3. ```dbservice``` - The service name for the database deployed on OpenShift
4. ```port```- The TCP port where the database server listens for incoming connections
5. ```dbname``` - The database name

All sensitive information must be hold in secrets. Make sure to .gitinore .env files and manually add secrets into RH.

If database is not in the same cluster we must replace with server IP address.

In this example we are using database templates. Similar things like perviously. Using PostgreSQL (Ephameral). Fill the fields like dbusername, password name and dbname.

In secrets we create to hold database connection secrets.

In example application make sure to change configuration string in ```config.js file``` replace with ```postgresql://<username>:<password>@<host>:<port>/<database>```

After new node.js deployment it should work. First it should be empty. If executing seed process you should see data in it if not you did something wrong.

Opening pod you can use sql queries in terminal. Similar with thing with k8s. Note that we usually don't make changes this way because it's danger. 

Running simple queries in terminal we can validate that seeding process is working properly.

## Chapter 4 - scaling applications

Okay interesting fact. If you have your database running in pod you cannot scaling it up. For that we need operators for doing so. **PS!In k8s we can use similar thing, but today we are using basic deployment and applications are not using operators**

Adn you cannot scale all pods up. Problem starts with state management. Also you can tell for single pod how much it can scale it self up min and max parameters. 

I assume LB will monitor the traffic and it it will up scale the pod. I think RH call LB -> Route??

