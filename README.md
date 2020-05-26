# helm-chart-grafana-agent

## Instructions:  

Update values.yaml with your URL, user name and password 

## Example run with Minikube: 

Start minikube and helm - ref: https://v2.helm.sh/docs/rbac/ 
```
minikube start 
kubectl config use-context minikube
kubectl create -f ./conf/rbac_config.yaml
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
