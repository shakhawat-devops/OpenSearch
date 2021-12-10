## OpenSearch as a Deployment in Kubernetes ##
If we do not want any store persistent we can install OpenSearch in Kubernetes cluster a
Deployment. Here I will show you the steps to do so. Let's begin

Deploy the yaml file by running <kubectl apply -f opensearch-depl.yaml>. It will deploy
1. A Deployment object for the OpenSearch cluster
2. A ClusterIP service for accessing the opensearch cluster
3. A Deployment object for the OpenSearch Dashboard
4. A Loadbalancer service for accessing the dashboard

Here I have used 3 replicas for the cluster. After deploying the objects your OpenSearch Cluster will be up and running.