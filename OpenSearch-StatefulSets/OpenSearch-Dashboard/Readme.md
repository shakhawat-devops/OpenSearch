######## OpenSearch Dashboard in Kubernetes ########
Congratulations! You have instlled the OpenSearch cluster and Now I will show how to deploy the 
Dashboard to view and access the cluster. 
Let's begin

In the OpenSearch-Dashboard repo you will find all the necessary files for the Dashboard

Please all of these resources will be created in the 'opensearch' namespace that was created in the previous deployment

1. First deploy the serviceaccount. <kubectl apply -f serviceaccount.yaml>
2. Then deploy the rolebinding. <kubectl apply -f rolebinding.yaml>
3. Now deploy the service file. I am deploying a ClusterIP service as the dashboard will be accessed by the ingress. 
Run <kubectl apply -f service.yaml> to deploy the ClusterIP service. 
Now deploy the dashboard as a Deployment object by running <kubectl apply -f deployment.yaml>
It will deploy the Dashboard with a single replica the it will be accessed by the nginx ingress controller. 
In my cluster I have deployed an Nginx Ingress Controller and I will use the host based routing to route traffic to the Dashboard. The dashboard will be found at "opensearch.eastnetic.com". Here is the traffic flow

public internet > ingress-controller > OpenSearch Dashboard > OpenSearch Cluster

To achive this I have written a ingress resource file now we will deploy this in your cluster. 
Run <kubectl apply -f ingress.yaml> and done. 

Hit the browser by typing "opensearch.eastnetic.com" and boom! You will see the login page of the opensearch cluster. Please note that for on-prem cluster you need to add a host entry in your host file mentioning your ingress controller ip to the fqdn of opensearch. In my case it is
192.168.0.240  "opensearch.eastnetic.com"

And with this the OpenSearch Stack is successfully installed in your Kubernetes Cluster!