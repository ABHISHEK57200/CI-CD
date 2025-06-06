# CI-CD
#Docker Containerization
FROM php:8.0-apache
COPY . /var/www/html
EXPOSE 80

#GitHub Actions for CI/CD

- name: Build and push Docker image
  run: |
    docker build -t ${{ secrets.DOCKER_USERNAME }}/mystore:latest .
    docker push ${{ secrets.DOCKER_USERNAME }}/mystore:latest
  #Kubernetes Deployment
  image: yourdockerhubusername/mystore:latest

#deployment.yaml
  apiVersion: apps/v1
kind: Deployment
metadata:
  name: mystore
spec:
  replicas: 2
  selector:
    matchLabels:
      app: mystore
  template:
    metadata:
      labels:
        app: mystore
    spec:
      containers:
      - name: mystore
        image: yourdockerhubusername/mystore:latest
        ports:
        - containerPort: 80


       # service.yaml
       apiVersion: v1
kind: Service
metadata:
  name: mystore-service
spec:
  type: NodePort
  selector:
    app: mystore
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30008


