# Note
## Step 1, Create Build Config
### Create the bc.yaml
```
kind: "BuildConfig"
apiVersion: "v1"
metadata:
  name: "cx-backend-build" 
spec:
  source: 
    type: "Git"
    git:
      uri: "https://github.com/suchan-redhat/golangtest.git"
    sourceSecret:
      name: "basicsecret"
  strategy: 
    type: Docker
    dockerStrategy:
      dockerfilePath: Dockerfile
  output: 
    to:
      kind: "DockerImage"
      name: "registry.cx-sunrise.openshift.com:443/sunny-demo3/aml_b2c-pilot_backend:latest"
    pushSecret:
      name: "dockerhub"
```
### Update the basicsecret with github login credential
```
oc secrets new-basicauth basicsecret --username=username --password=*****
```
### Update dockerhub secret to allow pushing image to registry
```
oc secrets new dockerhub ~/.docker/config.json
or
oc secrets new dockerhub ~/.dockercfg

oc secrets add serviceaccount/builder secrets/dockerhub
```
### create the buidlconfig
```
oc start-build cx-backend-build 
```

### run the build

## Step 2, Create Deployment Config
### template yaml

## Step 3, Create Service 
## Step 4, Create Route
## Step 5, Do all these in Jenkins

* oc login @@@@
* oc whoami -t
* docker login -u xxx -p $(oc whoami)
* oc secrets new dockerhub ~/.dockercfg
* or 
* oc secrets new dockerhub ~/.docker/config.json
* oc secrets add serviceaccount/builder secrets/dockerhub
* "pushSecret":{
*         "name": "dockerhub"
*      }

* oc policy add-role-to-user edit system:serviceaccount:sunny-test:default -n sunny-test
* oc get project
* oc project sunny-test
* oc describe serviceaccount default
* oc describe secret default-token-56u9n
