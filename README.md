Dockerization---
steps :
 1. clone the repository:

 bash
 git clone https://github.com/nyrahul/wisecow.git

 create a dockerfile:

 dockerfile

 # Use an official Node.js runtime as a base image (assuming it's a Node.js app)
 FROM node:16-alpine

 # Set the working directory in the container
 WORKDIR /app

 # Copy package.json and package-lock.json to the container
 COPY package*.json ./

 # Install dependencies
 RUN npm install --only=production

 # Copy the rest of the application code to the container
 COPY . .

 # Expose the port on which the app will run
 EXPOSE 3000

 # Start the application
 CMD ["npm", "start"]

 steps:

 build the image:

 bash
 docker build -t wise cow-app: latest

 test it locally:

 bash
 docker run -p 3000:3000 wise cow-app:latest

 kubernetes deployment:

 deployment.yaml:

 apiVersion: apps/v1
 kind: Deployment
 metadata:
   name: wise cow-deployment
 spec:
   replicas: 2
   selector:
     matchLabels:
       app: wise cow
   template:
     metadata:
       labels:
         app: wise cow
     spec:
       containers:
       - name: wise cow-app
         image: your-registry/wise cow-app:latest
         ports:
         - containerPort: 3000
         env:
         - name: NODE_ENV
           value: "production"

service.yaml:
yaml

apiVersion: v1
kind: Service
metadata:
  name: wise cow-service
spec:
  selector:
    app: wise cow
  ports:
    - protocol: TCP
      port: 80
      targetPort: 3000
  type: LoadBalancer

CI/CD with github actions:

.github/workflows/docker-deploy.yml

yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Docker Build x
      uses: docker/setup-buildx-action@v1

    - name: Log in to DockerHub
      uses: docker/login-action@v2
      with:
        username: ${{ secrets.DOCKER_USERNAME }}
        password: ${{ secrets.DOCKER_PASSWORD }}

    - name: Build and push Docker image
      run: |
        docker build -t your-dockerhub-username/wise cow-app:latest .
        docker push your-dockerhub-username/wise cow-app:latest

  deploy:
    runs-on: ubuntu-latest
    needs: build

    steps:
    - name: Deploy to Kubernetes
      uses: apple boy/k8s-action@v1.0.0
      with:
        kube config: ${{ secrets.KUBE CONFIG }}
        manifests: |
          deployment.yaml
          service.yaml

ingress.yaml:
yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: wise cow-ingress
  annotations:
    cert-manager.io/cluster-issuer: "lets encrypt-prod"
spec:
  rules:
  - host: wise cow.your domain.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: wise cow-service
            port:
              number: 80
  tls:
  - hosts:
    - wise cow.your domain.com
    secretName: wise cow-tls

cert-manager
bash
kubectl apply -f https://github.com/jetstack/cert-manger/releases/download/v1.7.0/cert-manager.yaml

apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: lets encrypt-prod
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: your-email@example.com
    privateKeySecretRef:
      name: lets encrypt-prod
    solvers:
    - http01:
        ingress:
          class: nginx

 
