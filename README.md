# paytm-insider project

Clone the repository and run the below command on a kubernetes cluster:
 - Change the deployment.yaml files to add your AWS credentials so that it can pick up the nodejs-test:latest image from the repository and run 10 replicas.
 - Run **kubectl create -f deployment.yaml** to pull the image and create a pod.
 - Run **kubectl create -f deployment-hpa.yaml** to create a Horizontal Pod Autoscaler which will autoscale the pods whenever the CPU load reaches 50% and memory load reaches 60%. Also, this manifest file will make sure that the node-js pods have higher priority than daemonset pods. This will also make sure that atleast 7 replicas are running all the time. 
 - Run **kubectl create -f service.yaml** to expose the pods using an EC2 Classic Load Balancer on 3000 port.
 - For running the load on the application, use the below:
   - Run **kubectl run --generator=run-pod/v1 -it --rm load-generator --image=busybox /bin/sh** to run a busybox shell so as to increase the load on the node-js pods.
   - When the shell for busybox opens, run **while true; do wget -1 -O- master-ip:service-port-exposed; done** This will load the app by hitting it multiple times.
