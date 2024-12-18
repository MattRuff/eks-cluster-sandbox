All of this is based off of this repo here:

GitHub - MattRuff/eks-cluster-sandbox at master 

 

Go to /terraform here: 

Install Terraform and run:



terraform plan
terraform apply
 

Connect to EKS Cluster via kubectl 



aws eks --region <region> update-kubeconfig --name <cluster_name>
Run: 



helm install my-release ./example-voting-app
Install dd agent

a. run:



helm repo add datadog https://helm.datadoghq.com
helm install datadog-operator datadog/datadog-operator
kubectl create secret generic datadog-secret --from-literal api-key=XXXXXXX
b. create datadog.yaml



apiVersion: "datadoghq.com/v2alpha1"
kind: "DatadogAgent"
metadata:
  name: "datadog"
spec:
  global:
    clusterName: {{insert-cluster-name-here w/ no Capitals!!}}
    credentials:
      apiSecret:
        secretName: "datadog-secret"
        keyName: "api-key"
  features:
    apm:
      instrumentation:
        enabled: true
        libVersions:
          java: "1"
          dotnet: "3"
          python: "2"
          js: "5"
    logCollection:
      enabled: true
      containerCollectAll: true
c. run:



kubectl apply -f datadog-agent.yaml
 

go to /example-voting-app



helm install my-release .
 

 
