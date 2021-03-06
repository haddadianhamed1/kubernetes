# test locally

step 1) build docker image
docker build -t hamed-node-minikube-kops .

# check the image
docker images

# run locally
docker run -p 3000:3000 -i -t hamed-node-minikube-kops

# test the site
localhost:3000
# we should get
Hello World!

# check for current context to see if current is kops or minikube
kubectl config view
kubectl config get-contexts
kubectl config set current-context minikube

#check for nodes
kubectl get nodes


# check for pods
kubectl get pods
kubectl delete pods --all



# check for deployments
kubectl get deployments
kubectl delete deployments --all


# check for replicasets
kubectl get rs
kubectl delete rs --all
# step A2)
Tag image for dockerhub
docker login
docker tag imageid your-login/docker-demo
docker tag 781a891e16ba hhaddadian/hamed-node-minikube-kops

push image to docker hub
docker push hhaddadian/hamed-node-minikube-kops



Step2 ) minikube

Option 1)
kubectl run hame-node-test --image=hhaddadian/hamed-node-minikube-kops --port=3000kubectl get pods
kubectl get pods
kubectl get deployments
kubectl expose deployment hame-node-test --type=NodePort
minikube service hame-node-test --url
or
minikube service list

Option 2)
A) Create pod-node-minikube.yml file
apiVersion: v1
kind: Pod
metadata:
  name: hamed-node.minikube.com
  labels:
    app: hamed-node-app
spec:
  containers:
  - name: hamednodejsminikube-container
    image: hhaddadian/hamed-node-minikube-kops
    ports:
    - containerPort: 3000


B) Create the pods
kubectl create -f pod-node-minikube.yml

C) check the pod
kubectl get pods
kubectl describe pods

# to get to our application use one of the following:
A) PortForward
kubectl port-forward hamed-node.minikube.com 8081:3000 # this will hold until we have the session after Ctrl+c we lose
B) Create a service
kubectl expose pod hamed-node.minikube.com --type=NodePort --name hamedminikube # we need --name because we cant use pod name with all the dashes
# to connect to service
A) minikube service list
or
B) minikube service hamedminikube --url


kubectl run hamed-node-minikube --image=hamed-node-minikube-kops --port=3000

kubectl get pods
