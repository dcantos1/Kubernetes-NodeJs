# Node-Microservice
microservice

Registrar ecr
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin URI de repo de ECR ejemplo:

aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 257654204247.dkr.ecr.us-east-1.amazonaws.com/01-k8s-aws

construir imagen: docker build -t 01-k8s-aws .
ver imagen: docker images
tagear imagen con URI de ECR: docker tag 01-k8s-aws:latest 257654204247.dkr.ecr.us-east-1.amazonaws.com/01-k8s-aws:latest
Por ultimo hacer un docker push a la imagen: docker push 257654204247.dkr.ecr.us-east-1.amazonaws.com/01-k8s-aws

Registrar el cluster de EKS para las configuraciones con kubectl
aws eks --region us-east-1 update-kubeconfig --name 01-k8s-aws (Nombre cluster)

Realizar el deploy en el cluster eks:
kubectl create -f deploy.yml 

Realizar el deploy del servicio en el cluster eks:
kubectl create -f serviceNodePort.yml  (se pasa una vez que este creado el deploy)

Modificar las replicas
kubectl scale --replicas=1 -f deploy.yml

Modificar
kubectl apply -f deploy.yml

revisar el despliegue:
kubectl get pods
kubectl get pods --watch
kubectl get all

Borrar pod
kubectl delete pod/nombrepod

Obtener servicios del contenedor
kubectl get service

Obtener el deploy
kubectl get deployments

Servicio Nodeport para exponer un puerto en especifico al publico.
El servicio mas usado es el de loadbalance, tambien esta el de clusterIP es para  exponer la IP del cluster para tener un control hacia los contenedores


problemas doc
https://repost.aws/knowledge-center/eks-api-server-unauthorized-error

Programa Lens control de kubernetes desktop
