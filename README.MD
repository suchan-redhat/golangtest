# Note
oc login @@@@
oc whoami -t
docker login -u xxx -p $(oc whoami)
oc secrets new dockerhub ~/.dockercfg
or 
oc secrets new dockerhub ~/.docker/config.json
oc secrets add serviceaccount/builder secrets/dockerhub
"pushSecret":{
        "name": "dockerhub"
      }

