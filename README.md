# helm-chart-grafana-agent

## Instructions:  

Update values.yaml with your Hosted Metrics / Prometheus URL, user_name and password 

## Quickstart running on Minikube: 

Start minikube and create helm service acc with cluster-admin role 
```
#
minikube start 
kubectl config use-context minikube
#
# ref: https://v2.helm.sh/docs/rbac/ 
wget https://gist.githubusercontent.com/aengusrooneygrafana/f481f4c5ebf350595a3fdb5a92130002/raw/711573ea173e60783d9c98a069cf7dee6c7edf5a/rbac-config.yaml 
kubectl create -f ./rbac_config.yaml
helm init --service-account tiller --history-max 200 --upgrade
```

Start chartmuseum - ref: https://chartmuseum.com/docs/#installation 
```
nohup chartmuseum --debug --port=8080 --storage="local" --storage-local-rootdir="./chartstorage” &  
```

Build chart for agent and load up to Chartmuseum  
```
mkdir ~/<path>/helm-chart-grafana-agent && cd ~/<path>/helm-chart-grafana-agent 
git clone https://github.com/aengusrooneygrafana/helm-chart-grafana-agent/
helm package ~/<path>/helm-chart-grafana-agent
curl --data-binary "@grafana-agent-0.1.1.tgz" http://localhost:8080/api/charts 
helm repo update 
helm search chartmuseum 
```

Start grafana agent 
```
helm install chartmuseum/grafana-agent -n grafana-agent-test 
```
