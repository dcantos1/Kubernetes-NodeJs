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

borrar deploy o servicio
kubectl delete -f deploy.yml
kubectl delete -f serviceNodePort.yml

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

eliminar ingress
kubectl delete ingress testing-kube-ingress


Instalar ingress
kubectl apply -k "github.com/aws/eks-charts/stable/aws-load-balancer-controller//crds?ref=master"
No olvidar lo permisos necesarios
kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/aws-alb-ingress-controller/v1.1.8/docs/examples/alb-ingress-controller.yaml


Servicio Nodeport para exponer un puerto en especifico al publico.
El servicio mas usado es el de loadbalance, tambien esta el de clusterIP es para  exponer la IP del cluster para tener un control hacia los contenedores


problemas doc
https://repost.aws/knowledge-center/eks-api-server-unauthorized-error

Programa Lens control de kubernetes desktop

terraform.exe plan -var-file terraform.tfvars (Para correr sobre un archivo de variables parametrizado)
terraform plan -var-file="terraform.tfvars"
terraform apply -var-file="terraform.tfvars" -auto-approve

https://kubernetes.github.io/ingress-nginx/deploy/
https://learnk8s.io/terraform-eks

editar finalizers en ingress y elimarlo.
kubectl patch ingress testing-kube-ingress -p '{"metadata":{"finalizers":[]}}' --type=merge 
editar finalizer en el service
kubectl patch service testing-kube-service -p '{"metadata":{"finalizers":[]}}' --type=merge

Otra soluciones
kubectl delete ValidatingWebhookConfiguration aws-load-balancer-webhook

Tag en VPC para ALB.
subnet Private
key                                 | value
kubernetes.io/role/internal-elb     | 1
kubernetes.io/cluster/nombreCluster | shared

subnet Public
key                                 | value
kubernetes.io/role/elb              | 1
kubernetes.io/cluster/nombreCluster | shared