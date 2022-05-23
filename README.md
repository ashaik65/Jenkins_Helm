# Jenkins

### Install Helm

```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

### Install and configure Jenkins

This documentation helps you to install and configure Jenkins


Create Namespace

```bash
k create ns jenkins
```

### Add Helm Repo

```bash
helm repo add jenkins https://charts.jenkins.io
helm repo update
helm search repo jenkins
```

```bash
NAME           	CHART VERSION	APP VERSION	DESCRIPTION                                       
jenkins/jenkins	4.1.1        	2.332.3    	Jenkins - Build great things at any scale! The ...
```
if you want to change something in values.yaml file use below cmd to fetch values.yaml file on local

```bash
helm show values jenkins/jenkins > /tmp/jenkins.yaml
```

### Install the chart

for any specific version with overridefile use below cmd

```bash
helm install jenkins jenkins/jenkins -f override.yml -n jenkins --version 3.3.12
```

or

```bash
helm install jenkins jenkins/jenkins -n jenkins
```

Get your 'admin' user password by running:

```bash
kubectl exec --namespace jenkins -it svc/jenkins -c jenkins -- /bin/cat /run/secrets/chart-admin-password && echo
```

either you can deycrypt the jenkins secret as well (username and password )

### Get the Jenkins URL 
1. via loadbalancer service type
2. using port-forward command if cluster is deployed on local

```bash
kubectl --namespace jenkins port-forward svc/jenkins 8080:8080
```
Login with the password from step 1 and the username: admin

# kaniko deployment

### create docker credentials where your jenkins has deployed i.e jenkins namespace

```bash
kubectl create secret docker-registry docker-credentials --docker-username=[userid] --docker-password=[Docker Hub access token] --docker-email=[user email address] --namespace jenkins
```

For more details : https://docs.cloudbees.com/docs/cloudbees-ci/latest/cloud-admin-guide/using-kaniko
