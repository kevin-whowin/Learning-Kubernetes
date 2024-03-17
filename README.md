# Learning Kubernetes

## Objectives

Run Kubernetes on your local machine

1. Create a GitHub repo
2. Install Minikube
3. Deploy an Nginx server on your local Kubernetes cluster
4. Visit the website in your browser

## Installation

1. Install Minikube:
   - Follow the installation instructions for your operating system from the official Minikube documentation: https://minikube.sigs.k8s.io/docs/start/

2. Start Minikube:
   - Open a terminal and run the following command to start Minikube:
     ```
     minikube start
     ```

3. Create a Deployment:
   - Create a file named `nginx-deployment.yaml` with the following content:
     ```yaml
     apiVersion: apps/v1
     kind: Deployment
     metadata:
       name: nginx-deployment
     spec:
       replicas: 1
       selector:
         matchLabels:
           app: nginx
       template:
         metadata:
           labels:
             app: nginx
         spec:
           containers:
           - name: nginx
             image: nginx:latest
             ports:
             - containerPort: 80
     ```
   - Run the following command to create the Deployment:
     ```
     kubectl apply -f nginx-deployment.yaml
     ```

4. Create a Service:
   - Create a file named `nginx-service.yaml` with the following content:
     ```yaml
     apiVersion: v1
     kind: Service
     metadata:
       name: nginx-service
     spec:
       selector:
         app: nginx
       ports:
       - port: 80
         targetPort: 80
       type: LoadBalancer
     ```
   - Run the following command to create the Service:
     ```
     kubectl apply -f nginx-service.yaml
     ```

5. Access the Nginx server:
   - Run the following command to get the URL of the Nginx server:
     ```
     minikube service nginx-service --url
     ```
   - Open the URL in your web browser, and you should see the default Nginx welcome page.

6. Customize the Nginx configuration (optional):
   - If you want to customize the Nginx configuration, you can create a ConfigMap with your desired configuration.
   - Create a file named `nginx-configmap.yaml` with the following content:
     ```yaml
     apiVersion: v1
     kind: ConfigMap
     metadata:
       name: nginx-config
     data:
       nginx.conf: |
         # Add your custom Nginx configuration here
     ```
   - Update the Deployment to mount the ConfigMap as a volume:
     ```yaml
     spec:
       containers:
       - name: nginx
         image: nginx:latest
         ports:
         - containerPort: 80
         volumeMounts:
         - name: config
           mountPath: /etc/nginx/nginx.conf
           subPath: nginx.conf
       volumes:
       - name: config
         configMap:
           name: nginx-config
     ```
   - Apply the updated Deployment and ConfigMap:
     ```
     kubectl apply -f nginx-configmap.yaml
     kubectl apply -f nginx-deployment.yaml
     ```

That's it! You now have an Nginx server running in Minikube. You can access it using the URL provided by the `minikube service` command.
