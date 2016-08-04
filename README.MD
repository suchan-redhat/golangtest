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
oc create -f bc.yaml 
```

### run the build
```
oc start-build cx-backend-build 
```

## Step 2, Create Deployment Config
### template yaml
```
kind: DeploymentConfig
apiVersion: "v1"
metadata:
  name: dc-b2c-pilot-backend
  labels:
    solution-centre.name: snm-aml
    product-team.name: aml
    product.name: b2c-pilot
    application.tier: backend
spec:
  replicas: 1
  template:
    metadata:
      labels:
        solution-centre.name: snm-aml
        product-team.name: aml
        product.name: b2c-pilot
        application.tier: backend
    spec:
      containers:
      - name: b2c-pilot-backend
        image: 172.30.166.216:5000/sunny-demo3/aml_b2c-pilot_backend:latest
        imagePullPolicy: Always
        ports:
        - containerPort: 5000
          protocol: TCP
        env:
        - name: POD_NAME
          valueFrom:
            fieldRef:
              fieldPath: metadata.name
        - name: POD_NAMESPACE
          valueFrom:
            fieldRef:
              fieldPath: metadata.namespace
        - name: POD_IP
          valueFrom:
            fieldRef:
              fieldPath: status.podIP
```
### Update Image source and tag

image: 172.30.166.216:5000/*sunny-demo3*/aml_b2c-pilot_backend:*latest*

### create the deployment config

```
oc create -f dc.yaml
```

## Step 3, Create Service 
### Service YAML

```
apiVersion: v1
kind: Service
metadata:
  name: svc-b2c-pilot-backend
  labels:
    solution-centre.name: snm-aml
    product-team.name: aml
    product.name: b2c-pilot
    application.tier: backend

spec:
  ports:
  - port: 5000
    protocol: TCP
  selector:
    solution-centre.name: snm-aml
    product-team.name: aml
    product.name: b2c-pilot
    application.tier: backend
```
### Create the service

```
oc create -f svc.yaml
```
## Step 4, Create Route
### Route YAML

```
apiVersion: v1
kind: Route
metadata:
  name: route-b2c-pilot-backend
spec:
  host: svc-b2c-pilot-backend-sunny-demo3.4d25.cx-sunrise.openshiftapps.com
  to:
    kind: Service
    name: svc-b2c-pilot-backend
```
### update the hostname
  host: *svc-b2c-pilot-backend-sunny-demo3*.4d25.cx-sunrise.openshiftapps.com

### Create the route

```
oc create -f route.yaml
```
## Step 5, Do all these in Jenkins
### To get the token

```
oc policy add-role-to-user edit system:serviceaccount:<project name>:default -n <project name>
oc project <project name>
oc describe serviceaccount default
oc describe secret <default token name>
copy the token part
```
### Keep the buildconfig and remove all the others
### insert Build Step of Type *Trigger OpenShift Build*
### insert Build Step of Type *Delete OpenShift Resource*
#### Input the YAML of dc.yaml
### insert Build Step of Type *Create OpenShift Resource*
#### Input the YAML of dc.yaml
### insert Build Step of Type *Delete OpenShift Resource*
#### Input the YAML of svc.yaml
### insert Build Step of Type *Create OpenShift Resource*
#### Input the YAML of svc.yaml
### insert Build Step of Type *Delete OpenShift Resource*
#### Input the YAML of route.yaml
### insert Build Step of Type *Create OpenShift Resource*
#### Input the YAML of route.yaml
