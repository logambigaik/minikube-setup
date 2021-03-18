# Prerequisite:

	To install Minikube, minimum two core is required.(Choose t2.medium)

# Minikube Cluster creation steps

# kubectl (install)
	curl -o kubectl https://amazon-eks.s3-us-west-2.amazonaws.com/1.14.6/2019-08-22/bin/linux/amd64/kubectl
	chmod +x ./kubectl
	mkdir -p $HOME/bin
	cp ./kubectl $HOME/bin/kubectl
	export PATH=$HOME/bin:$PATH
	echo 'export PATH=$HOME/bin:$PATH' >> ~/.bashrc
	source $HOME/.bashrc
	kubectl version --short --client

# Install docker:
	yum install docker -y
	service docker start

# Minikube setup:
	curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
	chmod +x minikube
	sudo mv minikube /usr/local/bin/
	yum install conntrack -y
	export PATH=/usr/local/bin:$PATH
	minikube start --driver=none

# Minikube Practice:

	$ minikube version
	$ minikube start --vm-driver=none
	$ kubectl get nodes
	$ kubectl create deployment --image=nginx nginx-deployment
        $ kubectl expose deployment nginx-deployment --port=80 --name=nginx-service --type=NodePort  for [serviceproxy)
	$ kubectl get deploy
	$ kubectl get pods --watch
    	$ kubectl get svc    ==> to check whatare all services running in our machine
	# To delete service:
	$ kubectl delete svc nginx-service
	$ kubectl delete deploy nginx-deployment 
	
![image](https://user-images.githubusercontent.com/54719289/111619476-898a5a80-87dd-11eb-9acd-be5e95d820b0.png)

	
![image](https://user-images.githubusercontent.com/54719289/111618683-965a7e80-87dc-11eb-9cd2-d7bba19941a9.png)


# WIth yaml file:
https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
	
	>> create yaml file
![image](https://user-images.githubusercontent.com/54719289/111621466-eedf4b00-87df-11eb-92c2-e07a8f27093e.png)

	>> kubectl create -f nginx-deploy.yaml
	>> kubectl get pods
	>> kubectl get deploy
	>> kubectl get svc 
	>> kubectl expose deployment nginx-deployment --port=80 --name=nginx-service --type=NodePort  for [serviceproxy)
        or create yaml for service
	
![image](https://user-images.githubusercontent.com/54719289/111622451-1da9f100-87e1-11eb-95db-0e991106bc69.png)
        
	>> kubectl create -f nginx-service.yml
	>> kubectl get svc
	
	
# To check the service deployment version we can use below command like:

	kubectl api-resources | get Deployment
	kubectl api-resources | get Service
	
	

		
