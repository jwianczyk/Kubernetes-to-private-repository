# Deploy our web application in K8s cluster from private Docker registry

The goal of this project is deployment of web application from private Docker registry on K8s cluster.
- `Kubernetes`
- `Helm`
- `Docker`
- `AWS EKS`
 
Project description:
- Create Secret for credentials for the `private Docker registry`
- Configure the `Docker registry secret` in application `Deployment` component
- Deploy web application image from our `private Docker registry `in `K8s cluster`


-------------------
Firstly we have to create secret with credentials for private repository, in my case it was AWS ECR. 
We can do that in two ways each having their own benefits.
- The first method is manually using `docker login` command and using the generated `.docker/config.json` file 
for creating a secret
- The second method requires using a single command for creating a k8s secret: \
`kubectl create secret docker-registry <name-of-secret>` \
`--docker-server=<ECR-address> `\
`--docker-username=AWS` \
`--docker-password=$(aws ecr get-login-password)`

The second method is more convenient but limits us to only one private repository per secret while the first one can 
contain credentials of more than one remote repositories.

After creating secret with remote repository credentials we pass the secret name into deployment along with name of 
repository so the kubernetes can pull the image into the cluster.
