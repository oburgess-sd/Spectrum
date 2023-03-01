You will need a docker repository to run the kafka-persistent-single.yaml file. 

Create the namespace

    kubectl create -f 'https://strimzi.io/install/latest?namespace=kafka' -n kafka

Create the Strimzi resources

    kubectl apply -f kafka-persistent-single.yaml -n kafka

can delete if required, using the following:

    kubectl -n kafka delete $(kubectl get strimzi -o name -n kafka)

you can push to your docker repo by creating a secret:

    kubectl create secret docker-registry yourdockerusername-registry-secret --docker-server=https://index.docker.io/v1/ --docker-username=yourusername --docker-password=xxxxxx --docker-email=youremail -n kafka

and then refer to this secret in the YAML (line 73 [pushSecret]):

    pushSecret: yourdockerusername-registry-secret

to add additional connectors, you need to add them to the plugins section first (name, URL, file type), and then copy lines 81 thru 99 to add 