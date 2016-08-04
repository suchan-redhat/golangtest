# Note
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
