# Solr Cloud Installation

## Install the Solr Operator

```bash
helm repo add apache-solr https://solr.apache.org/charts
helm repo update

# Install the Solr & Zookeeper CRDs
kubectl create -f https://solr.apache.org/operator/downloads/crds/v0.5.0/all-with-dependencies.yaml
# Install the Solr operator and Zookeeper Operator
helm install solr-operator apache-solr/solr-operator --version 0.5.0
```
## Install Solr Cloud

**Please** customize the manifest `deployment-solrCloud.yaml` before applying it to your kubernetes cluster.

```bash
kubectl apply -f deployment-solrCloud.yaml
```

The solr-operator has created a new resource type `solrclouds` which we can query
Check the status live as the deploy happens

```bash
kubectl get solrclouds -w
```

## Configure Solr for Entando

To be able to use SolrCloud with entando we need to create the `entando` collection:

```bash
curl "http://[namespace]-solr-solrcloud.[suffix_domain]/solr/admin/collections?action=CREATE&name=entando&numShards=1&replicationFactor=3&maxShardsPerNode=2&collection.configName=_default"
```

## References

- https://artifacthub.io/packages/helm/apache-solr/solr-operator