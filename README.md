# Prerequisite:

	Minikube is for single node, cant create Master/Node concept ( for testing and practising we can use it)

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
	
	

# selector and template labels:

![image](https://user-images.githubusercontent.com/54719289/111679791-6e3d4080-8819-11eb-8012-bee52772e1da.png)

# After troubleshoot:
![image](https://user-images.githubusercontent.com/54719289/111680262-f28fc380-8819-11eb-83a1-b5c39f147104.png)

# Practice with kubectl commands:

		kubectl get deploy
   		kubectl get pod
   		kubectl get nodes
   		kubectl get svc
   		kubectl api-resources | grep Deploy
   		kubectl api-resources | grep Deployment
   		kubectl api-resources | grep service

![image](https://user-images.githubusercontent.com/54719289/111696881-be71ce00-882c-11eb-8881-89b5d6e3a9cd.png)

		kubectl get pods --show-labels
		
![image](https://user-images.githubusercontent.com/54719289/111697788-e6156600-882d-11eb-987c-99acb3ef574c.png)

# To edit image after deploy:

 		kubectl set image deployment/nginx-deploy nginx=nginx:1.16.1 --record
		
		[root@mini ~]# kubectl set image deployment/nginx-deploy nginx=nginx:1.16.1 --record
		deployment.apps/nginx-deploy image updated



# deploy describe:

		kubectl describe deploy
		
![image](https://user-images.githubusercontent.com/54719289/111698785-2c1ef980-882f-11eb-99dc-eedfd37d680c.png)


# Rollback the changes:

		kubectl rollout status deployment/nginx-deploy
		
		kubectl rollout history deployment.v1.apps/nginx-deploy
		
		[root@mini ~]# kubectl rollout history deployment.v1.apps/nginx-deploy
		deployment.apps/nginx-deploy
		REVISION  CHANGE-CAUSE
		1         <none>
		2         kubectl set image deployment/nginx-deploy nginx=nginx:1.16.1 --record=true
		
		
		kubectl rollout history deployment.v1.apps/nginx-deployment --revision=2
![image](https://user-images.githubusercontent.com/54719289/111699868-93897900-8830-11eb-95af-0ff6acd4f9f2.png)


# Rolling Back to a Previous Revision

		kubectl rollout undo deployment.v1.apps/nginx-deploy
		
		[root@mini ~]# kubectl rollout undo deployment.v1.apps/nginx-deploy
		deployment.apps/nginx-deploy rolled back


		kubectl get deployment nginx-deploy
		
		[root@mini ~]# kubectl get deployment nginx-deploy
		NAME           READY   UP-TO-DATE   AVAILABLE   AGE
		nginx-deploy   2/2     2            2           43m
		
		kubectl describe deployment nginx-deploy
![image](https://user-images.githubusercontent.com/54719289/111701279-7fdf1200-8832-11eb-9476-7cc155bd4268.png)
		
		
# Scaling a Deployment 
	
		kubectl scale deployment.v1.apps/nginx-deploy --replicas=10

![image](https://user-images.githubusercontent.com/54719289/111701429-bf0d6300-8832-11eb-9ef1-ff059bc190e9.png)

		
		# Autoscale:
		
		kubectl autoscale deployment.v1.apps/nginx-deploy --min=4 --max=15 --cpu-percent=80
![image](https://user-images.githubusercontent.com/54719289/111702260-e3b60a80-8833-11eb-9032-9dea8551fcda.png)

		
		kubectl get deploy
		
		kubectl rollout pause deployment.v1.apps/nginx-deploy
		kubectl get rs   #rollout status
		
		kubectl rollout history deployment.v1.apps/nginx-deploy
		
![image](https://user-images.githubusercontent.com/54719289/111702633-7b1b5d80-8834-11eb-88f6-7f8d4d93a850.png)

		
		#REsource adding
		
		kubectl set resources deployment.v1.apps/nginx-deploy -c=nginx --limits=cpu=200m,memory=512Mi
		
		kubectl rollout resume deployment.v1.apps/nginx-deploy 
		
		kubectl get rs -w
		kubectl get rs
		
![image](https://user-images.githubusercontent.com/54719289/111702796-b87feb00-8834-11eb-8930-9fd29ce195d0.png)


		# kubectl command sets the spec with progressDeadlineSeconds to make the controller report lack of progress for a Deployment after 10 minutes
		
		kubectl patch deployment.v1.apps/nginx-deploy -p '{"spec":{"progressDeadlineSeconds":600}}'
		
		[root@mini ~]# kubectl patch deployment.v1.apps/nginx-deploy -p '{"spec":{"progressDeadlineSeconds":600}}'
		deployment.apps/nginx-deploy patched (no change)


![image](https://user-images.githubusercontent.com/54719289/111703132-21676300-8835-11eb-9ca1-db0c08d8be2d.png)





		












