# Kubernetes YAML files
1. To build and upload a custom container image:
`cd ../ && docker build -t famaten/my-public:my-tasky-v1.0 .` - a Docker Hub free repository registry
`docker push famaten/my-public:my-tasky-v1.0`

2. To create secret file for environment variables to make them available for Kubernetes POD
Run the command:
`kubectl create secret generic my-tasky-secret --from-env-file=my-tasky.env`

3. To make sure that container image has access to an arbitrary file named wizexercise.txt with specific content you may create configuration map 
`kubectl create cm my-tasky-cm --from-file=wizexercise.txt`
and mount the configuration map into container image (see deployment configuration from my-tasky-resources.yaml)

4. To create the rest of resources:
`kubectl apply -f my-tasky-resources.yaml`

5. To test connectivity you can learn dns name of the load balancer created by AWS under EXTERNAL-IP field (based on the service configuration out of my-tasky-resources.yaml file)
`kubectl get svc my-tasky-svc`

# Comments

- I have initially installed mongodb into Kubernetes cluster from helm chart for simplivity.
Obviously if mongodb is located out of the cluster (on a EC2 machine) my-tasky.env file and mongodb_uri variable have to be corrected. 
And we have make a connectivity between mongodb EC2 machine and AWS EKS managed node networks. 
(need to look into configuration of EKS cluster)

- There was no information about wizexercise.txt, therefore I have put a blank data inside of it.

- There was no specific information concerning required set of permissions for cluster role to be assigned to service account, therefore I have put a basic set of permissions: get, list, watch.