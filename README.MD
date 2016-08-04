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

* oc describe serviceaccount default
* oc describe secret default-token-05xnu
* oc policy add-role-to-user edit system:systemaccount:ci:default -n sunny-test
* oc policy add-role-to-user admin system:systemaccount:ci:default -n sunny-test
* oc describe serviceaccount default
* oc policy add-role-to-user edit system:systemaccount:ci:default -n sunny-test
