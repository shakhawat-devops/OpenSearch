## OpenSearch Installation in Kubernetes ##



In this github repo you will find the yaml files to install OpenSearch in Kubernetes. 
I will install a 3 node OpenSearch cluster as a StateFull Set and will NFS as my storage provisioner

I am assuming that you have the NFS Storage Class provisioned in your cluster already. 

Now Let's begin installing OpenSearch. 

1. Create a namespace named opensearch. <kubectl create ns opensearch>
2. Deploy the ConfigMap to the namespace. <kubectl apply -f configmap.yaml>
3. Now deploy the service for the opensearch. As we are deploying opensearch as a StatefulSet object in kubernetes, it needs a headless service for communicating with the other nodes of the deployment. In the service.yaml file you will get all the required services for the deployment. Now deploy using the kubectl command. <kubectl apply -f service.yaml>
4. After deploying all of them it is time to provision the actual server as a StatefulSet. Kubernetes StatefulSet object allows us to run statefull application in kubernetes. StatefulSets need a "volumeClaimTemplate" so that all the pods can have Persitent Volumes provisioned automatically using a StorageClass. In my project I have used a local NFS provisioner as my Storage Class and used that specific StorageClassName in my volumeClaimTemplate section. It can be different in terms of the kubernetes platform. In my case I am using an on-premise Kubernetes platform. If your kubernetes cluster is hosted on any cloud provider than the Storage Class will differ as per the cloud provider. 

Now deploy the the StatefulSet in the cluster. <kubectl apply -f statefulsets.yaml>

Now wait sometime so that the pods come up. After all the pods become running you can verify the configuration by running a curl command. 

Go inside the pod by typing <kubectl exec -it <pod-name> -n <namespace> -- bash>
Then type <curl -XGET https://localhost:9200 -u 'admin:admin' --insecure>

it will give a json output like the below one. 

####  
{ 
  "name" : "opensearch-cluster-master-0",
  "cluster_name" : "opensearch-cluster",
  "cluster_uuid" : "T_sMlwybQoW6seSwYarXsQ",
  "version" : {
    "distribution" : "opensearch",
    "number" : "1.2.0",
    "build_type" : "tar",
    "build_hash" : "c459282fd67ddb17dcc545ec9bcdc805880bcbec",
    "build_date" : "2021-11-22T16:57:18.360386Z",
    "build_snapshot" : false,
    "lucene_version" : "8.10.1",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "The OpenSearch Project: https://opensearch.org/"
}
#####

If you get this output then voila! Your OpenSearch Server has successfully set up as a StatefulSet in Kubernetes. Now I will deploy the OpenSearch Dashboard to access the OpenSearch Cluster. Head over to the other repository named OpenSearch-Dashboard
