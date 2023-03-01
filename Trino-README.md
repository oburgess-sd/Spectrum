# Installing trino
    helm repo add trino https://trinodb.github.io/charts

Create namespace:

    kubectl create namespace trino

Followed by:
    
    helm install trino-cluster trino/trino -n trino

Use this to set-up port forwarding to Trino

    kubectl port-forward --namespace trino svc/trino-cluster 8080:8080

Access the 